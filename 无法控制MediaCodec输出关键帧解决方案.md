在做视频录制时，发现使用MediaCodec做硬解码时，即使设置了MediaFormat的*MediaFormat.KEY_I_FRAME_INTERVAL*属性也无法控制输出Buffer中关键帧的输出数量。  
  后来发现原来是真正的原因是在于视频的**输入源**，如果是通过Camera的PreviewCallback的方式来获取视频数据再喂给MediaCodec的方式是无法控制输出关键帧的数量的。  
   想要控制输出输出关键帧数量就必须通过调用MediaCodec.createInputSurface()方法获取输入Surface，再通过Opengl渲染后喂给MediaCodec才能真正控制关键帧的数量*(至于为什么会这样我也没搞明白，希望有明白的能指教一下)*。
 > 判断输出数据是否为关键帧的方法：
 > boolean keyFrame = (bufferInfo.flags & MediaCodec.BUFFER_FLAG_KEY_FRAME) != 0;
