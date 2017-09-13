#### 问题描述：   

视频录制框架是仿照grafika来写的，大概流程是先创建编码器的输入Surface然后:采集手机摄像头原始yuv数据 —> 绘制到GLSurfaceView —> onDrawFrame(绘制一帧数据) —> eglSwapBuffers（交换数据，交给encoder编码） —> videoEncoder.dequeueOutputBuffer —> 循环以上步骤。。。    
 
 但是在某些手机上多次录制时，会出现录制失败问题，发现是由于在调用eglSwapBuffers方法时被阻塞了！
 
 #### 解决方案：   
 
 原来造成eglSwapBuffers方法被阻塞是由于BufferQueue满了所以造成了阻塞，而造成BufferQueue满了的原因是由于视频编码和音频编码都是在同一个线程中，由于在获取音频编码器的输入buffer时设置的超时时间是-1也就是无限等待（AudioEncoder.dequeueInputBuffer(-1)），所以造成编码线程一直阻塞在这个方法中，从而导致了视频编码器不能及时的处理eglSwapBuffers交换后的视频数据，所以也就导致了BufferQueue满了导致eglSwapBuffers方法阻塞。