> 1. 首先进入shell，打开命令行终端，输入：
~~~ java
		adb shell
~~~

> 2. 进入指定app的data目录:
~~~ java
		//这里的com.you.package是指你要查看app的完整包名
		run-as com.you.package
~~~
>3. 进入到目录后，可以运行“ls”命令查看文件列表，如果想要将某个文件拷贝到电脑上可以使用以下命令:
~~~ java
	//mydata.db是指你要复制的文件 /sdcard/是指要复制到的路径
	cp mydata.db /sdcard/
~~~
>4. 退出adb shell或重新打开一个命令行窗口：
~~~ java
		//sdcard/mydata.db是指刚才文件复制后的绝对目录
		//Documents/ 是指要拷贝到电脑上的目录
		adb pull /sdcard/mydata.db Documents/
~~~
>5. 拷贝到电脑后可以通过命令行的方式"sqlite3"查看也可以通过工具查看，往下就不写具体的了。