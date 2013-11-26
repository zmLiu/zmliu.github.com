---
layout : post
title : Swf一键导出到Starling中的工具，在Starling使用原生的MovieClip
tags : [Tool]
---

***有些同学使用过程用遇到了问题。我开了一个QQ群。有问题的同学可以进来问一下群号是168436154***

## 目录 ##
 ***1.*** StarlingSwf是什么 
 
 ***2.*** 功能与特色 
 
 ***3.*** 下载与安装 
 
 ***4.*** 教程一：资源命名规则 
 
 ***5.*** 教程二：针对设计师 
 
 ***6.*** 教程三：针对程序 
 
 ***7.*** 教程四：MovieClip自动停止播放 
 
 ***8.*** 教程五：获取界面上的元素
 


----------
## StarlingSwf是什么 ##

StarlingSwf是一套开源的 Swf数据导出到Starling框架中使用的工具.

他可以让开发导出Swf数据到Starling中渲染


----------
## 功能与特色 ##
 ***1.*** 导出Swf数据到Starling中
 
 ***2.*** 在Starling中还原Swf中原件的层级关系、动画、原件属性
 
 ***3.*** 支持原件嵌套，动画嵌套
 
 ***4.*** MovieClip基本还原了传统Flash的MovieClip
 
 ***5.*** 使用了类似骨骼动画的思想，内存占用低、运行效率高
 
 ***6.*** 自动合并纹理，并且可以自动单独导出大图

----------
## 下载与安装 ##

[工具源码地址][1]

[依赖库地址][2]

[Mac版下载][3]

[Windows版下载][4]

[Demo][5]

[Demo中Fla的兼容版本][6]


  [1]: https://github.com/zmLiu/StarlingSWF
  [2]: https://github.com/zmLiu/StarlingFeathers
  [3]: http://url.cn/U3rH7S
  [4]: http://url.cn/N7tHHQ
  [5]: http://url.cn/RfP3mV
  [6]: http://url.cn/JCViDo
  


----------
## 教程一：原件命名规则 ##
 
 ***既然是Swf那么资源的编辑肯定还是用Flash Pro了，但是资源的命名规则大家需要注意下(`是AS链接名称噢`)***

 ***1.*** img 开始会被识别为starling.display.Image.**(`这个必须是原件。不能直接用图片`,导出之后该原件会直接被映射为一张图片)**
 
 ***2.*** s9  开头会被识别为feathers.display.Scale9Image.**(制作规则跟传统flash开发一样)**
 
 ***3.*** btn 开头会被识别为lzm.starling.display.Button.(**Button中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)
 
 ***4.*** mc  开头会被识别为lzm.starling.swf.display.SwfMovieClip.(**MoviecClip中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)
 
 ***5.*** spr 开头会被识别为starling.display.Sprite.(**Sprite中使用的任意显示对象必须是`img` `s9` `btn` `mc` `spr`中制作的对象**)

 ***6.*** 文本 文本比较特殊 只要在`img` `s9` `btn` `mc` `spr` 中写就可以了
 


----------
## 教程二：针对设计师 ##

 ***1.***作为设计师，你只需要准备好需要展示的图片，在FlashPro中将他们有序的组装起来。并且为需要导出的原件设置链接就可以了搞定一切

 ***2.***打开库面板，你可以看到此示例的相关资源.
 
<img src="/assets/images/starling_swf_tool/image1.png" alt="截图" class="img-rounded">

 ***3.***相关资源编辑好之后导出Swf.

 ***4.***打开StarlingSwf导出工具，选择刚刚导出的swf.选择上方的下拉框，预览在Swf内部的原件在Starling中是什么样子.
 
<img src="/assets/images/starling_swf_tool/image2.png" alt="截图" class="img-rounded">

 ***5.***选择导出倍数，然后导出.资源导出后会出现两个文件夹
 
<img src="/assets/images/starling_swf_tool/image3.png" alt="截图" class="img-rounded">

 ***data下面放的Swf数据,images下放的所有导出的图片.将这两个东西给程序就可以了.***


----------
## 教程三：针对程序 ##

 ***1.***建立一个Actionscript工程。(demo中是手机工程)
 
<img src="/assets/images/starling_swf_tool/image4.png" alt="截图" class="img-rounded">

 ***2.***倒入资源
 
<img src="/assets/images/starling_swf_tool/image5.png" alt="截图" class="img-rounded">

 ***3.***倒入依赖库
 
<img src="/assets/images/starling_swf_tool/image6.png" alt="截图" class="img-rounded">



 ***4.***代码
 
***关键代码***
 	
 	//初始化Swf
 	Swf.init(Starling.current.nativeStage);
 	
 	//创建一个Swf
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
    	import flash.display.StageAlign;
    	import flash.display.StageScaleMode;
    	
    	import lzm.starling.STLStarup;
    	
    	public class StarlingSwfTestMain extends STLStarup
    	{
    		public function StarlingSwfTestMain()
    		{
    			super();
    			
    			// 支持 autoOrient
    			stage.align = StageAlign.TOP_LEFT;
    			stage.scaleMode = StageScaleMode.NO_SCALE;
    			stage.color = 0x999999;
    			stage.frameRate = 60;
    			
    			initStarling(StarlingSwfTestMainClass);
    		}
    	}
    }
    
    package
    {
    	import flash.filesystem.File;
    	
    	import lzm.starling.STLConstant;
    	import lzm.starling.STLMainClass;
    	import lzm.starling.gestures.DragGestures;
    	import lzm.starling.swf.Swf;
    	
    	import starling.core.Starling;
    	import starling.display.Sprite;
    	import starling.text.TextField;
    	import starling.utils.AssetManager;
    	import starling.utils.formatString;
    	
    	public class StarlingSwfTestMainClass extends STLMainClass
    	{
    		
    		private var textfield:TextField;
    		
    		public function StarlingSwfTestMainClass()
    		{
    			super();
    			
    			Swf.init(Starling.current.nativeStage);
    			
    			textfield = new TextField(200,100,"loading....");
    			textfield.x = (STLConstant.StageWidth - textfield.width)/2;
    			textfield.y = (STLConstant.StageHeight - textfield.height)/2;
    			addChild(textfield);
    			
    			var assets:AssetManager = new AssetManager(STLConstant.scale,STLConstant.useMipMaps);
    			var file:File = File.applicationDirectory;
    			
    			assets.enqueue(file.resolvePath(formatString("assets/{0}x/",STLConstant.scale)));
    			assets.loadQueue(function(ratio:Number):void{
    				textfield.text = "loading...." + int(ratio*100)+"%";
    				if(ratio == 1){
    					textfield.removeFromParent(true);
    					
    					var swf:Swf = new Swf(assets.getByteArray("layout"),assets);
    					
    					var sprite:Sprite = swf.createSprite("spr_1");
    					addChild(sprite);
    					
    					new DragGestures(sprite);
    				}
    			});
    		}
    	}
    }

 ***5.***运行效果
 
 <img src="/assets/images/starling_swf_tool/image7.png" alt="截图" class="img-rounded">
 
 
  


----------

## 教程四：MovieClip自动停止播放 ##

 ***很多时候需要让动画播放完毕之后自动停止在最后一帧，在工具中也可以很简单的实现***
 
 ***1.*** 倒入swf
 
 ***2.*** 从MovieClip列表中 选中需要自动停止的MC
 
 ***3.*** 取消`是否循环`
 
 <img src="/assets/images/starling_swf_tool/image9.png" alt="截图" class="img-rounded">
   


----------

## 教程五：获取界面上的元素 ##

 很简单，首先给元素命名
 
 <img src="/assets/images/starling_swf_tool/image8.png" alt="截图" class="img-rounded">
 
 然后使用`getChildByName`获取
    


----------
 
***友情提示：工具的预览区域是可以拖动的噢***



	