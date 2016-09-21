# Windows设置开机穹顶

## 运行打开“启动”文件夹的命令

win+R

1. 打开“系统启动文件夹”的命令： `shell:Common Startup`， 对应目录 `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`
2. 打开“用户启动文件夹”的命令： `shell:startup`, 对应目录 `C:\Users\Zebin\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`

将要启动的程序拖到里面就可以了


如果是手动打开这个目录， ProgramData 是隐藏文件夹，应当现在文件夹选项中设置为显示隐藏的文件夹才能够进入。

## 删除开机启动

win+R --> msconfig --> 启动

或者

win8以上可以直接在任务管理器中管理启动项