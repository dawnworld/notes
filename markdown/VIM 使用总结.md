VIM 使用总结
--
#### 1. 行内快速移动

| Command      |     Comment |
| :-------- | --------:|
| `f` `x`    |   向`前`移动到`x`字符`上` | 
| `F` `x`    |   向`后`移动到`x`字符`上` | 
| `t` `x`    |   向`前`移动到`x`字符`前` | 
| `T` `x`    |   向`后`移动到`x`字符`前` | 

#### 2. 一些常用命令
``` sh
:%s/[Ctrl + V][Ctrl + M]//g #去除所有的^M
```