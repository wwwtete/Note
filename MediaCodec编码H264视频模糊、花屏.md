  问题描述：在给视频加一下特效后二次合成编码时，发现在某些手机上二次编码后的视频出现严重的模糊、花屏问题，但是原视频也是同一个手机录制的编码就没问题，只是在二次编辑后才会出现这个问题     
    解决方案：  
	最后发现是由于**MediaFormat.KEY_BITRATE_MODE**设置的参数问题导致的，默认录制视频时设置的参数值为： **MediaCodecInfo.EncoderCapabilities.BITRATE_MODE_VBR**是没问题的，但是如果录制编码成h264后再解码做二次操作后再次编码成h264时就会在有些手机上编出来的视频模糊、花屏，后来改为： **MediaCodecInfo.EncoderCapabilities.BITRATE_MODE_CQ**就可以了！    
	关于**MediaFormat.KEY_BITRATE_MODE**有3种可选值       
	
- BITRATE_MODE_CQ: 表示完全不控制码率，尽最大可能保证图像质量
- BITRATE_MODE_CBR: 表示编码器会尽量把输出码率控制为设定值  
- BITRATE_MODE_VBR: 表示编码器会根据图像内容的复杂度（实际上是帧间变化量的大小）来动态调整输出码率，图像复杂则码率高，图像简单则码率低；
