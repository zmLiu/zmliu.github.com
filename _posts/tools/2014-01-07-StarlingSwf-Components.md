---
layout : post
title : StarlingSwf 自定义组件
tags : [Tool]
---
***有些同学使用过程用遇到了问题。我开了一个QQ群。有问题的同学可以进来问一下群号是168436154***

[StarlingSwf](http://zmliu.github.io/2013/11/09/StarlingSwfTool/)

----------

####自定义组件是为了能够轻松的添加一些有特定功能的组件

####也可以轻松的把现有的ui框架集成到程序中

####比方说`Feathers`的组件 通过`StarlingSwf`的自定义组件 就可以在Flash pro中编辑并布局啦

#教程

***1、组件的链接命名规则***

以`comp`开头就会被识别为组件。但是单单以`comp`开头是不科学的。比方说的要定制一个`feathers`的`button`那么组件的链接名称可以命名为`comp_feathers_btn`这个看个人喜好。但是必须以`comp`开头



----------

***2、按照一定的规则编辑Flash pro中的元件***

这个部分的规则，由开发组件的 开发者定义。




----------

***3、编写组件的类文件***

`StarlingSwf`的自定义组件都需要实现`ISwfComponent`这个接口。然后实现接口中的`initialization`方法即可

但是很多时候，可能需要在fla中制作一个公用组件。然后各处引用 这样就满足不了一些需求了 所以：这里引入了组件可编辑属性了机制

只要实现`editableProperties`方法 工具就能让你编辑组件属性


----------

***4、在组件配置列表中加入配置组件***

组件的类文件编辑好了。接下来在`ComponentConfig`这个类中配置一下就可以动态的创建组件了



----------

***5、例子***

这里我使用`feathers`的`button`简单的编写一个组件

先了解清楚`button`展示的构成部分,`feathers`的`button`我这里简单的设置他的三个状态

	1.普通状态
	2.鼠标按下状态
	3.鼠标抬起的状态
	
准备资源

	我直接使用了`feathers`官方的资源,并且在fla中编辑好，如图:
	
<img src="/assets/images/starling_swf_tool_components/image1.png" alt="截图" class="img-rounded">

编辑fla中的组件

首先新建一个元件链接名字命名为如图：

<img src="/assets/images/starling_swf_tool_components/image2.png" alt="截图" class="img-rounded">


然后编辑相应的显示对象：

<img src="/assets/images/starling_swf_tool_components/image3.png" alt="截图" class="img-rounded">
<img src="/assets/images/starling_swf_tool_components/image4.png" alt="截图" class="img-rounded">
<img src="/assets/images/starling_swf_tool_components/image5.png" alt="截图" class="img-rounded">

组件类文件编写

	package lzm.starling.swf.components.feathers
	{
		import flash.text.TextFormat;
		
		import feathers.controls.Button;
		
		import lzm.starling.swf.components.ISwfComponent;
		import lzm.starling.swf.display.SwfSprite;
		
		import starling.display.DisplayObject;
		import starling.text.TextField;
	
		public class FeathersButton extends Button implements ISwfComponent
		{
			
			public function initialization(componetContent:SwfSprite):void{
				var _upSkin:DisplayObject = componetContent.getChildByName("_upSkin");
				var _selectUpSkin:DisplayObject = componetContent.getChildByName("_selectUpSkin");
				var _downSkin:DisplayObject = componetContent.getChildByName("_downSkin");
				var _disabledSkin:DisplayObject = componetContent.getChildByName("_disabledSkin");
				var _selectDisabledSkin:DisplayObject = componetContent.getChildByName("_selectDisabledSkin");
				
				var _labelTextField:TextField = componetContent.getTextField("_labelTextField");
				
				this.defaultSkin = _upSkin;
				if(_selectUpSkin) this.defaultSelectedSkin = _selectUpSkin;
				if(_downSkin) this.downSkin = _downSkin;
				if(_disabledSkin) this.disabledSkin = _disabledSkin;
				if(_selectDisabledSkin) this.selectedDisabledSkin = _selectDisabledSkin;
				
				if(_labelTextField){
					var textFormat:TextFormat = new TextFormat();
					textFormat.font = _labelTextField.fontName;
					textFormat.size = _labelTextField.fontSize;
					textFormat.color = _labelTextField.color;
					textFormat.bold = _labelTextField.bold;
					textFormat.italic = _labelTextField.italic;
					
					this.defaultLabelProperties.textFormat = textFormat;
					this.label = _labelTextField.text;
				}
				
				componetContent.removeFromParent(true);
			}
			
			public function get editableProperties():Object{
				return {
					label:label,
					isEnabled:isEnabled
				};
			}
			
			public function set editableProperties(properties:Object):void{
				for(var key:String in properties){
					this[key] = properties[key];
				}
			}
			
		}
	}


配置组件

打开`lzm.starling.swf.components.ComponentConfig`找到`componentClass`,添加对应的组件

	private static var componentClass:Object = {
		"comp_feathers_btn":FeathersButton
		"其他":其他
	};

----------

以上操作完成 组件就添加完毕啦。

以代码方式启动工具 就能在工具中预览了。

如果不是以代码方式启动工具。那么工具中 组件会以`sprite`的方式呈现，不会有组件的特性。就需要到自己的工程里面 创建swf然后预览了


----------

例子中的fla在[这里](https://github.com/zmLiu/StarlingSWF/tree/0.0.7/StarlingSWF-Test/testFla)

最后来一张预览截图

<img src="/assets/images/starling_swf_tool_components/image6.png" alt="截图" class="img-rounded">