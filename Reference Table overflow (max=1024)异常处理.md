　　首先说一下出现这个问题的背景，通过Android的Camera采集视频信息然后通过JNI来调用C来软编码，但是发现有的手机再录制时间超过5分钟后就会出现异常崩溃！通过抓log发现是：“*JNI pinned array reference table (0x5d4440a8) dump; ReferenceTable overflow (max=1024)*”引起的奔溃！
  　　其中 ==ReferenceTable overflow (max=1024)== 这句log是最关键的，它指出是由于引用计数器溢出造成的崩溃，看到这里后我排查了JNI代码，果然是JNI代码处理问题,因为我每次调用JNI方法时都会调用GetByteArrayElements来接受byte数组，但是却一直没有释放
~~~ java
uint8_t *inputBuffer = (uint8_t *) env->GetByteArrayElements(inputBuffer_, 0);
~~~    
解决方案就是JNI方法中用完一行一定记得要释放,调用: ==env->ReleaseByteArrayElements(jbyteArray array, jbyte* elems,
        jint mode)==
### 总结一下JNI中经常遇到的问题：
> 1. 忘记释放引用或释放内存         

　　凡是用到**New**的方法都需要手动进行释放(如:*env->NewByteArray*)，调用: ==env->DeleteLocalRef==方法进行释放,
还有调用**GetByteArrayELement***方法也要手动释放，调用:==env->ReleaseTypeElements==方法进行释放，如果只是取bytearray中的byte可以使用==GetByteArrayRegion==方法来获取  
> 2.发生Reference Table overflow (max=1024) 或 Reference Table overflow (max=512)之类的异常

　　如果发生类似的异常，就去排查JNI的代码，肯定有未释放的引用(*global reference、local reference*)
>3.多线程的问题
 
 　　第一种情况:在多线程使用JNIEnv*对象，需要AttachCurrentThread将env挂到当前线程，否则无法使用env
  　　第二种情况:在多线程中调用java方法，需要保存jobject对象，这时需要对jobject对象做全局引用(==NewGlobalRef==)，否则会失效
>4.在JNI层获取jbytearray长度

 　　不应该在JNI层获取jbytearray长度，应该在java层获取byte数组长度，然后再传给JNI层
