　　首先说一下出现这个问题的背景，通过Android的Camera采集视频信息然后通过JNI来调用C来软编码，但是发现有的手机再录制时间超过5分钟后就会出现异常崩溃！通过抓log发现是：“*JNI pinned array reference table (0x5d4440a8) dump; ReferenceTable overflow (max=1024)*”引起的奔溃！
  　　其中 ==ReferenceTable overflow (max=1024)== 这句log是最关键的，它指出是由于引用计数器溢出造成的崩溃，看到这里后我排查了JNI代码，果然是JNI代码处理问题,因为我每次调用JNI方法时都会调用GetByteArrayElements来接受byte数组，但是却一直没有释放
~~~ java
uint8_t *inputBuffer = (uint8_t *) env->GetByteArrayElements(inputBuffer_, 0);
~~~    
解决方案就是JNI方法中用完一行一定记得要释放,可以调用: ==env->ReleaseByteArrayElements(jbyteArray array, jbyte* elems,
        jint mode)==

