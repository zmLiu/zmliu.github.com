---
layout : post
title : Swf一键导出到Starling中的工具，在Starling使用原生的MovieClip
tags : [Tool]
---

***有些同学使用过程用遇到了问题。我开了一个QQ群。有问题的同学可以进来问一下群号是168436154***

----------
## 目录 ##
 ***1.*** [StarlingSwf是什么](#whatThis)
 
 ***2.*** [功能与特色](#featuresFunctions)
 
 ***3.*** <a id="downInstall" href="http://zmliu.github.io/2013/12/17/StarlingSwfToolUpdate/" target="_blank">下载与安装</a>
 
 ***4.*** [教程一：资源命名规则](#Tutorials1)
 
 ***5.*** [教程二：针对设计师](#Tutorials2)
 
 ***6.*** [教程三：针对程序](#Tutorials3)
 
 ***7.*** [教程四：MovieClip自动停止播放](#Tutorials4)
 
 ***8.*** [教程五：获取界面上的元素](#Tutorials5)
 
 ***9.*** [教程六：ATF批量导出工具如何使用](#Tutorials6)
 
 ***10.*** <a href="http://zmliu.github.io/2014/01/07/StarlingSwf-Components/" target="_blank">自定义组件</a>
 
 ***11.*** <a href="http://zmliu.github.io/2014/03/27/StarlingSwf-Particle/" target="_blank">粒子集成教程</a>
 
 ***12.*** [成功案例](#SuccessStories)
 


----------
<h2 id="whatThis">StarlingSwf是什么</h2>

StarlingSwf是一套开源的 Swf数据导出到Starling框架中使用的工具.

他可以让开发导出Swf数据到Starling中渲染


----------
<h2 id="featuresFunctions">功能与特色</h2>

 ***1.*** 导出Swf数据到Starling中
 
 ***2.*** 在Starling中还原Swf中原件的层级关系、动画、原件属性
 
 ***3.*** 支持原件嵌套，动画嵌套
 
 ***4.*** MovieClip基本还原了传统Flash的MovieClip
 
 ***5.*** 使用了类似骨骼动画的思想，内存占用低、运行效率高
 
 ***6.*** 自动合并纹理，并且可以自动单独导出大图

----------
## <a id="downInstall" href="http://zmliu.github.io/2013/12/17/StarlingSwfToolUpdate/" target="_blank">下载与安装(点我)</a> ##  


----------
<h2 id="Tutorials1">教程一：原件命名规则</h2>
 
 ***既然是Swf那么资源的编辑肯定还是用Flash Pro了，但是资源的命名规则大家需要注意下(`是AS链接名称噢`)***

 ***1.*** img 开始会被识别为starling.display.Image.**(`这个必须是元件。不能直接用图片`,导出之后该原件会直接被映射为一张图片)**
 
 ***2.*** s9  开头会被识别为feathers.display.Scale9Image.**(制作规则跟传统flash开发一样)**
 
 ***3.*** btn 开头会被识别为lzm.starling.display.Button.(**Button中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)
 
 ***4.*** mc  开头会被识别为lzm.starling.swf.display.SwfMovieClip.(**MoviecClip中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)
 
 ***5.*** spr 开头会被识别为starling.display.Sprite.(**Sprite中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)

 ***6.*** 文本 文本比较特殊 只要在`img` `s9` `btn` `mc` `spr` 中写就可以了
 
 ***7.*** shapeImg 开头会被识别为lzm.starling.swf.display.ShapeImage,使用纹理填充的图片(纹理长宽需要为2的幂数,改变组建宽高时，会自动使用初始化的Texture填充) 
 
 ***8.*** comp 开头会被识别为带特殊功能的组件
 
 ***9.*** particle 开头会被识别为粒子(需要制作规范。具体请看粒子集成教程)
 


----------
<h2 id="Tutorials2">教程二：针对设计师</h2>

 ***1.***作为设计师，你只需要准备好需要展示的图片，在FlashPro中将他们有序的组装起来。并且为需要导出的原件设置链接就可以了搞定一切

 ***2.***打开库面板，你可以看到此示例的相关资源.
 
<img src="/assets/images/starling_swf_tool/image1.png" alt="截图" class="img-rounded">

 ***3.***相关资源编辑好之后导出Swf.

 ***4.***打开StarlingSwf导出工具，选择刚刚导出的swf.选择上方的下拉框，预览在Swf内部的原件在Starling中是什么样子.
 
<img src="/assets/images/starling_swf_tool/image2.png" alt="截图" class="img-rounded">

 ***5.***导出。点击导出按钮。导出之后会在导出目录下，以选择的Swf名字，创建一个文件，文件夹内部存放了Swf导出之后的数据
 
<img src="/assets/images/starling_swf_tool/image3.png" alt="截图" class="img-rounded">


----------
<h2 id="Tutorials3">教程三：针对程序</h2>

 ***1.***建立一个Actionscript工程。(demo中是手机工程)
 
<img src="/assets/images/starling_swf_tool/image4.png" alt="截图" class="img-rounded">

 ***2.***倒入资源
 
<img src="/assets/images/starling_swf_tool/image5.png" alt="截图" class="img-rounded">

 ***3.***倒入依赖库
 
<img src="/assets/images/starling_swf_tool/image6.png" alt="截图" class="img-rounded">



 ***4.***代码
 
***关键代码***
 	
 	//初始化Swf
 	Swf.init(Starling的根容器);
 	
 	//创建一个Swf(`layout`对应生成`.bytes`文件的名字)
 	var swf:Swf = new Swf(assets.getByteArray("layout"),assets);
 	
 	//根据as连接名称创建 显示对象
 	swf.createSprite("spr_1");
 	swf.createMovieClip("mc_test1");
 	swf.createImage("img_test1");
 	swf.createButton("btn_test1");
 	swf.createS9Image("s9_test1");
 	
***demo中的主要代码***
    
    package
	{
		import flash.filesystem.File;
		
		import lzm.starling.STLConstant;
		import lzm.starling.STLMainClass;
		import lzm.starling.swf.Swf;
		import lzm.starling.swf.SwfAssetManager;
		
		import starling.display.Sprite;
		import starling.text.TextField;
		import starling.utils.formatString;
		
		public class TestMainClass extends STLMainClass
		{
			
			private var textfield:TextField;
			private var assets:SwfAssetManager;
			
			public function TestMainClass()
			{
				super();
				
				Swf.init(this);
				
				textfield = new TextField(200,100,"loading....");
				textfield.x = (STLConstant.StageWidth - textfield.width)/2;
				textfield.y = (STLConstant.StageHeight - textfield.height)/2;
				addChild(textfield);
				
				assets = new SwfAssetManager(STLConstant.scale,STLConstant.useMipMaps);
				assets.verbose = true;
				var file:File = File.applicationDirectory;
				
				assets.enqueue("test",[file.resolvePath(formatString("assets/{0}x/test/",STLConstant.scale))],60);
				assets.loadQueue(function(ratio:Number):void{
					textfield.text = "loading...." + int(ratio*100)+"%";
					if(ratio == 1){
						textfield.removeFromParent(true);
						
						test1();
	//					test2();
					}
				});
			}
			
			private function test1():void{
				var sprite:Sprite = assets.createSprite("spr_1");
				addChild(sprite);
			}
			
			private function test2():void{
				var sprite:Sprite = assets.createSprite("spr_particle");
				addChild(sprite);
			}
		}
	}

 ***5.***运行效果
 
 <img src="/assets/images/starling_swf_tool/image7.png" alt="截图" class="img-rounded">
 
 
  


----------

<h2 id="Tutorials4">教程四：MovieClip自动停止播放</h2>

 ***很多时候需要让动画播放完毕之后自动停止在最后一帧，在工具中也可以很简单的实现***
 
 ***1.*** 倒入swf
 
 ***2.*** 从MovieClip列表中 选中需要自动停止的MC
 
 ***3.*** 取消`是否循环`
 
 <img src="/assets/images/starling_swf_tool/image9.png" alt="截图" class="img-rounded">
   


----------

<h2 id="Tutorials5">教程五：获取界面上的元素</h2>

 很简单，首先给元素命名
 
 <img src="/assets/images/starling_swf_tool/image8.png" alt="截图" class="img-rounded">
 
 然后使用`getChildByName`获取
    


----------

<h2 id="Tutorials6">教程六：ATF批量导出工具如何使用</h2>

####[移步到这里](http://zmliu.github.io/2013/08/31/ATFTool/)
    


----------

<h2 id="SuccessStories">成功案例</h2>

####[战略传奇](http://ng.d.cn/zhanlvechuanqi/)
####[船长也疯狂](https://itunes.apple.com/cn/app/chuan-zhang-ye-feng-kuang/id597315819?mt=8)
####[天天爆爆](http://img.tonlo.com/ttbb/download.html)
####[Description](https://itunes.apple.com/us/app/gu-ying-jian-ke/id726049617?ls=1&mt=8)

----------

 
***友情提示：工具的预览区域是可以拖动的噢***



	