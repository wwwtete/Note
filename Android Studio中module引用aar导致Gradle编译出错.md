
 ### - 引用aar的方法
 

 1. 把aar文件放到 libs 目录下面，并且在对应的 module 项目下面 build.gralde 中添加如下配置:
 ~~~ java
 repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
	// 其中aar-file-name不用后缀名
	compile(name: 'aar-file-name', ext: 'aar')  
}
 ~~~