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

  [1]: https://www.jianshu.com/p/7be526b77bd2