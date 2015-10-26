---
layout: post
title: "小米状态栏颜色"
description: ""
category: 
tags: []
---
{% include JB/setup %}

小米的系统，最上方的状态栏是没有任何背景颜色的，当切换主题的时候就会遇到问题。当系统是白色的时候，如果切换到一个白色背景的主题，会导致上边状态栏的内容是分辨不出来的。（魅族的状态栏也没有颜色，但是他会根据当前背景，自动更新颜色，所以没有这个问题）

这里通过反射的方式寻得了仅针对小米系统的一个解决方案:

<pre>
/**
 * 判断是否是小米，如果是的话，通过反射的方式，修改状态栏模式。
 * @param window
 */
public static void statusBar(Window window) {
    Boolean blackbg = Res.getBoolean(R.bool.isPop);
    Class clazz = window.getClass();
    try {
        int tranceFlag = 0;
        int darkModeFlag = 0;
        Class layoutParams = Class.forName("android.view.MiuiWindowManager$LayoutParams");

        Field field = layoutParams.getField("EXTRA_FLAG_STATUS_BAR_TRANSPARENT");
        tranceFlag = field.getInt(layoutParams);

        field = layoutParams.getField("EXTRA_FLAG_STATUS_BAR_DARK_MODE");
        darkModeFlag = field.getInt(layoutParams);

        Method extraFlagField = clazz.getMethod("setExtraFlags", int.class, int.class);

        extraFlagField.invoke(window, blackbg ? tranceFlag : darkModeFlag, blackbg ? tranceFlag : darkModeFlag);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
</pre>

