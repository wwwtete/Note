> 1.在你的主app的 build.gradle文件中添加对native源码库的引用
~~~ java
dependencies {
		debugCompile project(path: ':源码库', configuration: 'debug')
	    releaseCompile project(path: ':源码库', configuration: 'release')
}
~~~
> 2.修改native源码库的 build.gradle文件以启用publishNonDefault
~~~ java
android {
    publishNonDefault  true
}
~~~