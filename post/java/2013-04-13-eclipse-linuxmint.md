{
layout: "pages",
title: "eclipse在linux mint下的界面调整",
category: "java",
description: "调整eclipse在linux mint的方式",
tags: ["java","eclipse"]
}

本文主要参考[eclipse在ubuntu下面的界面调整](http://hchaojie.iteye.com/blog/904145)。

eclipse目录下面创建文件ec-gtkrc:

```gtk-style
style "eclipse_ui" {   
	xthickness=1	
	ythickness=1  
	GtkButton::default_border={1,1,1,1}	
	GtkButton::default_outside_border={1,1,1,1}	
	GtkButtonBox::child_min_width=0	
	GtkButtonBox::child_min_heigth=0	
	GtkButtonBox::child_internal_pad_x=0	
	GtkButtonBox::child_internal_pad_y=0	
	GtkMenu::vertical-padding=1	
	GtkMenuBar::internal_padding=1	
	GtkMenuItem::horizontal_padding=2	
	GtkToolbar::internal-padding=1	
	GtkToolbar::space-size=1	
	GtkOptionMenu::indicator_size=0	
	GtkOptionMenu::indicator_spacing=0	
	GtkPaned::handle_size=4	
	GtkRange::trough_border=0	
	GtkRange::stepper_spacing=0	
	GtkScale::value_spacing=0	
	GtkScrolledWindow::scrollbar_spacing=0	
	GtkExpander::expander_size=10	
	GtkExpander::expander_spacing=0	
	GtkTreeView::vertical-separator=0	
	GtkTreeView::horizontal-separator=0	
	GtkTreeView::expander-size=10	
	GtkTreeView::fixed-height-mode=TRUE	
	GtkWidget::focus_padding=0	
	font_name="Monospace 8"	
}	
  
class "GtkWidget" style "eclipse_ui"	
class "GtkButton" style "eclipse_ui"	
class "GtkToolbar" style "eclipse_ui"	
class "GtkPaned" style "eclipse_ui"  
```

里面有个font_name="Monospace 8"就是调整界面大小的。

在桌面创建一个脚本:
```bash
#!/bin/sh
GTK2_RC_FILES=/opt/eclipse/ec-gtkrc /opt/eclipse/eclipse
```

这样你就可以在linux下看见小图标的eclipse了。
