---
title: mac-config
typora-root-url: mac-config
typora-copy-images-to: mac-config
date: 2019-08-21 14:36:53
tags:
---

1. VSCode 中Vim插件的Normal模式, 长按按键的时候, 不会连续触发. 

   ``` bash
   default write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
   default write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
   // 然后重启, 在用系统自带的输入法的时候会这样, 而souguo输入法不会限制连续输入.
   ```

   
   1. 安装 非认证开发者的软件的时候, mojave 会不予许, 按下`Control`后鼠标单击打开

