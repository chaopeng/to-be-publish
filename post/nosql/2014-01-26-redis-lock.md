{
layout: "pages",
title: "redis的分布式锁",
category: "nosql",
description: "redis实现分布式锁",
tags: ["分布式锁", "nosql", "redis"]
}

# 引子

redis是一个很强大的数据结构存储的nosql数据库，很方便针对业务模型进行效率的优化。最近我的工作是负责对现有Java服务器框架进行整理，并将网络层与逻辑层脱离，以便于逻辑层和网络层的横向扩展。
尽管我在逻辑层上使用了AKKA作为核心框架，尽可能lockfree，但是还是免不了需要跨jvm的锁。所以我需要实现一个分布式锁。

# 官方的实现
官方在[SETNX](http://redis.io/commands/setnx) 这一页给了一个实现。

* C4 sends SETNX lock.foo in order to acquire the lock
* The crashed client C3 still holds it, so Redis will reply with 0 to C4.
* C4 sends GET lock.foo to check if the lock expired. If it is not, it will sleep for some time and retry from the start.
* Instead, if the lock is expired because the Unix time at lock.foo is older than the current Unix time, C4 tries to perform: GETSET lock.foo (current Unix timestamp + lock timeout + 1)
* Because of the GETSET semantic, C4 can check if the old value stored at key is still an expired timestamp. If it is, the lock was acquired.
* If another client, for instance C5, was faster than C4 and acquired the lock with the GETSET operation, the C4 GETSET operation will return a non expired timestamp. C4 will simply restart from the first step. Note that even if C4 set the key a bit a few seconds in the future this is not a problem.

但是使用官方推荐的getset实现的话，未竞争到锁的一方确实可以判断到自己未能竞争到锁，但却将持有锁一方的时间修改了，这样的直接后果就是，持有锁的一方无法解锁！！！

# 基于lua的实现

其实官方实现出现的问题，是因为使用redis独立的命令不能将get-check-set这个过程进行原子化，所以我决定引入redis-lua，将get-check-set这个过程使用lua脚本来实现。

加锁：

* script params: lock_key, current_timestamp, lock_timeout
* setnx lock_key (current_timestamp + lock_timeout). if not success, set lock_key (current_timestamp + lock_timeout) if current_timestamp > value
* client save current_timestamp(lock_create_timestamp)

解锁：

* script params: lock_key, lock_create_timestamp, lock_timeout
* delete if lock_create_timestamp + lock_timeout == value

具体的实现:

```{lua}
---lock

local now = tonumber(ARGV[1])
local timeout = tonumber(ARGV[2])
local to = now + timeout
local locked = redis.call('SETNX', KEYS[1], to)
if (locked == 1) then
    return 0
end
local kt = redis.call('type', KEYS[1]);
if (kt['ok'] ~= 'string') then
    return 2
end
local keyValue = tonumber(redis.call('get', KEYS[1]))
if (now > keyValue) then
    redis.call('set', KEYS[1], to)
    return 0
end
return 1

---unlock

local begin = tonumber(ARGV[1])
local timeout = tonumber(ARGV[2])
local kt = redis.call('type', KEYS[1]);
if (kt['ok'] == 'string') then
    local keyValue = tonumber(redis.call('get', KEYS[1]))
    if ((keyValue - begin) == timeout) then
        redis.call('del', KEYS[1])
        return 0
    end
end
return 1
```

# 已知问题

redis的分布式锁会有单点的问题。当然我们的业务量也没有达到挂掉专门做锁的redis单点的水平。

