---
layout : post
title : Swf一键导出到Starling中的工具
tags : [Tool]
---

## 目录 ##
 ***1.*** StarlingSwf是什么 
 
 ***2.*** 功能与特色 
 
 ***3.*** 下载与安装 
 
 ***4.*** 教程一：资源命名规则 
 
 ***5.*** 教程二：针对设计师 
 
 ***6.*** 教程三：针对程序 
 


----------
## StarlingSwf是什么 ##

StarlingSwf是一套开源的 Swf数据导出到Starling框架中使用的工具.

他可以让开发导出Swf数据到Starling中渲染


----------
## 下载与安装 ##

[工具源码地址][1]

[依赖库地址][2]

[Mac版下载][3]

[Windows版下载][4]

[Demo][5]


  [1]: https://github.com/zmLiu/StarlingSWF
  [2]: https://github.com/zmLiu/StarlingFeathers
  [3]: http://url.cn/NFvIhb
  [4]: http://url.cn/MRuj6f
  [5]: http://url.cn/PYIIHb
  


----------
## 教程一：资源命名规则 ##

 ***1.*** img 开始会被识别为starling.display.Image.**(导出之后该原件会直接被映射为一张图片)**
 
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

 ***4.***打开StarlingSwf导出工具，选择刚刚导出的swf.可以预览在Swf内部的原件在Starling中是什么样子.
 
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


	