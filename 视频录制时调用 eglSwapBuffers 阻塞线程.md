#### 问题描述：   

视频录制框架是仿照grafika来写的，大概流程是先创建编码器的输入Surface然后:采集手机摄像头原始yuv数据 —> 绘制到GLSurfaceView —> onDrawFrame(绘制一帧数据) —> eglSwapBuffers（交换数据，交给encoder编码） —> videoEncoder.dequeueOutputBuffer —> 循环以上步骤。。。