Rockchip Power On Animation Image Pack Tool V0.2.0 For RK2906 Eink SDK User Guide
Author: yzc@rock-chips.com
Date  : 2013-01-07

1、AnimationPacker概述
	AnimationPacker可以用来打包一些图片来作为2906Eink SDK的开机动画。
	AnimationPacker还可以打包一张开机显示低电的图片，你可以在AniPackerConfig.ini文件中配置它
	它会将一系列图片解码、去抖、裁剪、数据格式转换、压缩，生成一个静态数组，供kernel eink驱动调用。
	目前它仅仅支持jpeg、png、bmp(24位/32位)、gif作为输入图片。

2、使用方法：
   a、将制作好的动画图片，放到与RKAnimationPacker.exe同一个文件夹
   b、配置AniPackerConfig.ini文件，修改相应的参数
   c、执行RKAnimationPacker.exe
   d、如果执行成功，则会生成一个.c文件，供2906 eink SDK刷屏驱动使用。

3、AniPackerConfig.ini配置文件
	该文件可以用来配置AnimationPacker。请注意，配置输出文件以及输入文件名格式的时候，目前只支持英文路径，不支持中文的路径。
	具体配置的详细说明可以参看配置文件中的注释。

4、支持的命令行参数
	命令行参数的配置会覆盖从AniPackerConfig.ini读取的配置。以下是目前可用的命令行参数：
	
	-g: gray scale，配置灰度值，目前支持2, 4，8，16四种灰阶
	-n: frame count， 指定动画的帧数，打包工具会根据这个帧数，来打包图片。最小值为1，最大值为40
	-c: clip frame，是否对每一帧进行裁剪，0为不裁剪，1为裁剪，建议开启裁剪，减小数据量的大小
	-t: compress type，压缩类型，0不压缩，1为用zlib压缩，建议启用压缩，减小数据量的大小
	-o: output file name，指定输出文件的名字
	
5、debug模式
  在AniPackerConfig.ini文件中可以配置DebugMode来开启debug模式。在debug模式下，会有更详细的log，
  而且还会将中间处理的图片存成png文件供用户查看结果。

5、注意事项

	以下注意事项之前或有涉及，但在这里统一归纳一下：
		a、bmp格式的输入图片仅仅支持24位和32位
		b、每一张输入图片的大小必须是一致的，最好是全屏，低电图片可以任意大小。
		c、配置输出文件以及输入文件名格式的时候，目前只支持英文路径，不支持中文的路径。
		d、如果无法读取配置文件，则会使用默认参数。命令行参数的配置会覆盖从AniPackerConfig.ini读取的配置。
		e、工具会先打包开机动画图片，在根据需要打包低电图片。
		f、最后请将生成的RKPowerOnAnimation.c文件拷贝至kernel/drivers/video/ebc/bootani/ 目录下。重新编译kernel。

		
6、ChangeLog

Version         Release Time                Log
V0.2.0          2013-01-23                  1、增加dither配置开关，默认为关闭 2、增加debug模式
V0.1.5          2013-01-21                  增加低电图灰阶的配置，解决图片毛刺问题 
V0.1.4          2013-01-18                  1、解决2灰阶的bug.  2、取消低电图片的大小限制. 3、readme.txt文档更新. 4、更新AniPackerConfig.ini文件中的说明
V0.1.2          2013-01-08                  初始版本                     