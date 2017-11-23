---
layout:  post
title: "Programming languages(coursera)"
date: 2017-11-18 20:40:00 +0800
categories: blog
tags: other
---

#### EMACS(optional)

- C-v  下一屏  
- M-v  上一屏  
- C-l  重绘(将光标位置所在行置于屏幕中间)
- C-p(previous)  上一行  
- C-n(next)  下一行  
- C-b(backward)  左移  
- C-f(forward)  右移  
- M-b  左移一个词  
- M-f  右移一个词  
- C-a  移动到行首
- C-e  移动到行尾
- M-a  移动到句首  
- M-e  移动到句尾  
- M-<  开头  
- M->  结尾  
- C-u  前缀参数  
- C-g  终止指令  
- C-x 1 只保留当前窗格  
- < DEL>  删除光标前的一个字符  
- C-d  删除光标后的一个字符  
- M-< DEL>  移除光标前的一个字符  
- M-d  移除光标后的一个字符  
- C-k  移除到行尾的字符  
- M-k  移除到句尾的字符  
- C-<SPC>/C-@  选择模式 C-w 移除  
- C-y  召回(剪切-粘贴)  
- M-y  召回(轮选)  
- C-/  撤销  

文件  

- C-x C-f  查找(新建)文件  
- C-x C-s  保存文件  

缓冲区  

- C-x C-b  列出缓冲区  
- C-x b  切换缓冲区(C-x b TUTORIAL.cn)   
- C-x s  保存多个缓冲区  

命令集扩展  

- C-x  字符扩展(C-x之后输入另一个字符或者组合键)  
- M-x  命令名扩展(M-x之后输入一个命令名)  
- C-x u  撤销  
- C-x C-c  离开Emacs  
- M-x replace-string <RETURN>  进入替换模式  
- M-x recover file <RETURN>  恢复自动保存的文件  
- M-x fundamental-mode <RETURN>  切换到Fundamental模式  
- M-x text-mode <RETURN>  切换到Text模式  
- M-x auto-fill-mode <RETURN>  启动(关闭)自动折行模式  
- C-u 70 C-x f  设定70个字符换行  
- M-q  手动折行

搜索  

- C-s  向前搜索  
- C-r  向后搜索  

多窗格  

- C-x 2  将屏幕划分成为两个窗格
- C-M-v(ESC C-v)  滚动下方窗格  
- C-M-S-v  滚动上方窗格  
- C-x o  将光标移动到下一个窗格(遍历)  

多窗口  

- M-x make-frame <RETURN> 新建窗口  
- M-x delete-frame <RETURN>  关闭窗口  

递归编辑  

- ESC ESC ESC  离开递归编辑  


帮助  

- C-h m 查看当前主模式文档
- C-h c <组合键>  查看组合键功能  
- C-h k <组合键>  在新窗口中展示组合键文档  
- C-h f <函数名>  解释一个函数  
- C-h v <变量名>  显示变量文档  
- C-h a <关键词>  相关命令搜索  
- C-h i  阅读手册  

术语对照:  

<pre>
command             命令 
cursor              光标
scrolling           滚动
numeric argument    数字参数
window              窗格
insert              插入
delete              删除
kill                移除
yank                召回
undo                撤销
file                文件
buffer              缓冲区
minibuffer          小缓冲区
echo area           回显区
mode line           状态栏
search              搜索
incremental search  渐进式搜索
</pre>  

余下笔记[https://github.com/whhc/coursera-programming-languages/blob/master/Note/note-1.markdown](https://github.com/whhc/coursera-programming-languages/blob/master/Note/note-1.markdown)