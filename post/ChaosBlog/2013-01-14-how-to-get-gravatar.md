{
layout: "pages",
title: "怎样获取你gravatar的头像",
category: "ChaosBlog",
description: "通过api获取gravatar的头像的方法",
tags: ["ChaosBlog","gravatar"]
}

ChaosBlog一开始做的时候，头像是写死的靠上传来替换，但是由于完全静态，这样用起来相当不方便，所以还是用回gravatar的头像服务吧。

gravatar头像的地址
---

`头像服务器/avatar/邮箱的md5值?s=头像尺寸&d=默认头像&r=头像等级`

头像服务器
---

* http://www.gravatar.com
* http://0.gravatar.com
* http://1.gravatar.com
* http://2.gravatar.com
* http://3.gravatar.com
* http://s.gravatar.com
* http://en.gravatar.com
* https://secure.gravatar.com

据说在国内最稳定的是最后一个。

头像尺寸
---
可以选择1～512，即1～512px。

默认头像
---
默认头像这个参数我也没有试出来，但是据说是有这些可以选的：

* 留空 显示gravatar官方图形
* 404 直接返回404错误状态
* mm 神秘人(一个灰白头像)
* identicon 抽象几何图形
* monsterid 小怪物
* wavatar 用不同面孔和背景组合生成的头像
* retro 八位像素复古头像

头像等级
---

最后一个就是限制级的问题了，自己用gravatar的时候都知道，头像上传以后会审核分级的。

* g 适合任何年龄的访客查看，一般都用这个
* pg 可能有争议的头像，只适合13岁以上读者查看
* r 成人级，只适合17岁以上成人查看
* x 最高等级，不适合大多数人查看

源代码
---

最后附上我的写的一个获取头像的类的吧，此类基于apache lisence开源，请尊重版权。

```java
/** @author chao */
public class GravatarUtils {
	/**
	 * @param server
	 * @param email
	 * @param d
	 * - 留空 显示gravatar官方图形
	 * - 404 直接返回404错误状态
	 * - mm 神秘人(一个灰白头像)
	 * - identicon 抽象几何图形
	 * - monsterid 小怪物
	 * - wavatar 用不同面孔和背景组合生成的头像
	 * - retro 八位像素复古头像
	 * @param size (1~512)
	 * @param rate (g, pg, r, x) 默认g
	 */
	public static String getAvatar(String server, String email, String size, String d, String rate){
		StringBuilder sb = new StringBuilder(server).append("/avatar/");
		sb.append(Md5Utils.getMD5(email.toLowerCase()));
		sb.append("?s=").append(size);
		if(d!=null && d.length()!=0){
			sb.append("&d=").append(d);
		}
		if(rate!=null && rate.length()!=0){
			sb.append("&r=").append(rate);
		}
		return sb.toString();
	}
	
	public static String getAvatar(String server, String email, String size){
		return getAvatar(server, email, size, "", "");
	}
}
```
