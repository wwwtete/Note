   在上一篇文章[Android Hook工具之Frida 安装配置教程
][1] 中讲解了如何安装配置Frida工作环境，这篇文章主要讲解一下Frida的基础使用方式.
 > 在运行以下任何命令之前必须先启动手机中的frida-server
 
- 使用*frida-trace*命令跟踪某个特定的函数:
`frida-trace -U -i [函数名] [程序包名]`
例: 跟踪Chrome中的open函数，先在手机中启动Chrome，否则会跟踪失败，启动后，在终端窗口中输入*frida-trace*命令
``` stylus
frida-trace -U -i open com.android.chrome
Instrumenting functions...                                              
open: Auto-generated handler at "/Users/wangw/unapk/frida/__handlers__/libc.so/open.js"
Started tracing 1 function. Press Ctrl+C to stop. 
```
跟踪成功后，会在当前路径下创建以下JavaScript文件(/Users/wangw/unapk/frida/__handlers__/libc.so/open.js),Frida会将JavaScript文件中的代码注入到进程中并跟踪指定的函数调用，可以修改生成的JavaScript文件实现Hook功能，下面就是自动生成Js文件内容
``` stylus
{
  onEnter: function (log, args, state) {
    log("open(" +
      "path=\"" + Memory.readUtf8String(args[0]) + "\"" +
      ", oflag=" + args[1] +
    ")");
  },

  onLeave: function (log, retval, state) {
  }
}
```
跟踪成功后就会打印出Chrome调用open函数每一次调用，输出结果类似下面这样:
``` stylus
frida-trace -U -i open com.android.chrome
Instrumenting functions...                                              
open: Auto-generated handler at "/Users/wangw/unapk/frida/__handlers__/libc.so/open.js"
Started tracing 1 function. Press Ctrl+C to stop.                       
           /* TID 0x2252 */
  1312 ms  open(path="/data/user/0/com.android.chrome/code_cache/com.android.opengl.shaders_cache", oflag=0xc2)
  1314 ms  open(path="/data/user/0/com.android.chrome/code_cache/com.android.opengl.shaders_cache", oflag=0xc2)
           /* TID 0x2298 */
  3264 ms  open(path="/data/user/0/com.android.chrome/shared_prefs/com.android.chrome_preferences.xml", oflag=0x241)
```
- 强制启动一个应用进程
`frida -U --no-pause -f [应用包名]`
使用`-f`选项表示强制启动一个应用程序，`--no-pause`选项表示不中断应用程序的启动，如果不使用这个选项总是会遇到 在强制启动应用程序2秒左右后程序自动退出，
例: 强制启动Chrome并attach到当前进程
``` stylus
frida -U --no-pause -f com.android.chrome
     ____
    / _  |   Frida 10.6.52 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at http://www.frida.re/docs/home/
Spawned `com.android.chrome`. Resuming main thread!                     
[HUAWEI EVA-AL10::com.android.chrome]-> 
```
当attach成功后，就可以开始Hook Java函数和对象了。
关于Friada框架中Java部分的API可以查看[官方文档][2]，下面总结几个常用的API。
- `Java.available`  返回一个Boolean值，表示当前进程是否加载了 JVM也就是是否运行在Dalvik 或 ART环境中.如果返回false则Java 相关的API则无法调用
``` stylus
[HUAWEI EVA-AL10::com.android.chrome]-> Java.available
true
```
- `Java.enumerateLoadedClasses(callbacks)` 列出已加载的类
``` stylus
[HUAWEI EVA-AL10::com.android.chrome]-> Java.perform(function(){Java.enumerateLo
adedClasses({"onMatch":function(className){ console.log(className) },"onComplete
":function(){console.log(onComplete) }})})
org.chromium.chrome.browser.ntp.ContentSuggestionsNotifier
org.chromium.chrome.browser.snackbar.undo.UndoBarController
```
所有的代码逻辑都封装在`Java.perform(function(){ … }) `中，逻辑很简单，就是使用Java.enumerateLoadedClassesFridas API 列出所有已加载的类，并将每个类的className输出到控制台，这种使用方式是Frida框架中最常用的，它其实就是一个回调对象模板，
``` javascript
{
  "onMatch":function(arg1, ...){ ... },
  "onComplete":function(){ ... },
}
```
一旦Frida匹配到你的请求，就会使用一个或多个参数调用onMeth方法，如果匹配完成时就会调用onComplete函数



  [1]: https://www.jianshu.com/p/7be526b77bd2
  [2]: https://www.frida.re/docs/javascript-api/#java