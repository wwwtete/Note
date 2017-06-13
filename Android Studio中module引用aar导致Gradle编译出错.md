
 ###   引用aar的方法
 

 -  把aar文件放到 libs 目录下面，并且在对应的 module 项目下面 build.gralde 中添加如下配置:
 ~~~ java
 repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
	// 其中aar-file-name不用文件后缀名
	compile(name: 'aar-file-name', ext: 'aar')  
}
 ~~~
 
 ### 非主app项目中引用aar导致编译出错
  一般项目比较复杂的时候，都会有很多个 library 工程，可能会有多个module,如果在这些 library 工程中引入aar后，则编译的时候，可能会报错，一般错误信息为: **Failed to resolve:  aar-file-name**   
## 解决方案
如果要解决这个问题，其实就是要保证所有的 Module 都能引用到某个Module下面的 aar ，所以可以在最外层的Project 下的 build.gradle 中的 repositories 中添加引用即可！
**注意：这里build.gradle文件必须是最外层Project下的那个文件**
~~~ java
allprojects {
    repositories {
        jcenter()

        flatDir {
            dirs project(':引用aar的library名称').file('libs')  
        }
    }
}
~~~
  
  