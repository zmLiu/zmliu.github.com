---
layout : post
title : StarlingSwf UI管理扩展
tags : [Tool]
---

##下载##

[源码以及示例下载][1]

[StarlingSwf][2]

  [1]: http://url.cn/TsCF75
  
  [2]: http://zmliu.github.io/2013/11/09/StarlingSwfTool/
     


----------

该扩展的优势

该扩展主要针对手机开发，当然做页游也可以。
	
没有使用过传统swf做UI的可以忽略此处，大家刚开始使用starling的时候通常都是swf+starling进行开发，而这个时候你的UI好多功能在手机上的效果那就不是一个惨字能表示的~比如拖拽，随便拖一下就把你的帧数降到20帧以下，更不要提现在手机上流行的滚动拖拽页面了，而本工具以及StarlingSwf就针对这部分人群，相信已经玩的很转的swf做UI的你们接触应该不会很难，StarlingSwf将你熟悉的Swf以Starling的方式展现给你，而该扩展将在你熟悉的基础上给你更简单方便的UI制作。
	
内存方面：大家都知道手机的内存有限，很多时候UI不会进行缓存，然而UI使用完毕之后的销毁将会是一个头疼的问题，使用传统的swf做UI的话，swf文件有时候哪怕你存在了一个引用都不会进行销毁，而其他的场景有可能就差这么点内存就蹦了。该扩展销毁资源的时候采用强制销毁模式，销毁的是你加载的资源，哪怕你正在使用也会销毁。

使用之前的准备步骤：

首先呢，你需要了解StarlingSwf工具的基本使用。

你需要使用StarlingSwf将你的swf来进行导出，导出的3个文件建议放到一个文件夹下，

例如:

<img src="/assets/images/starling_swf_jiessie/image0.png" alt="截图" class="img-rounded">

该UI扩展需要你布置swf的时候稍微下点功夫，如下示例

<img src="/assets/images/starling_swf_jiessie/image1.png" alt="截图" class="img-rounded">

你需要将你的组件进行分组管理，也就是一个功能对应一个mc，当然这只是建议。

在程序中使用：

在使用该扩展之前你需要初始化两个地方：

Swf.init(stage); //只有初始化该信息才能使用SwfMovieClip

JISConfig.windowStage = starling.display.DisplayObjectContainer; //只有设置该值才能使用JISWindow

建立第一个Window

比如我们第一个Window命名为JISMainUIWindow

	//我们需要在构造函数中调用父类构造
	super(“spr_TestWindow”,”test”);
	//然后设置窗口资源
	setAssetSource(File.applicationDirectory.resolvePath("assets/ui/test/"));
	//调用show();
	show();
	这样window就开始加载并且在加载完毕之后显示了。

其中super的两个参数第一个为swf的导出链接名字

<img src="/assets/images/starling_swf_jiessie/image2.png" alt="截图" class="img-rounded">

第二个参数为文件的名字

<img src="/assets/images/starling_swf_jiessie/image0.png" alt="截图" class="img-rounded">

setAssetSource可以设置一个File，如果是文件夹的话，会扫描文件夹下所有文件，如果你的是web资源的话，你可以设置一个Url数组：`[“../assets/ui/test/test.bytes”, “../assets/ui/test/test.png”, “../assets/ui/test/test.xml”]`

如此，我们的第一个Window就创建好并能够显示了。

管理Window中的组件：

我们的window中有个组件名字为_BtnList,如图：

<img src="/assets/images/starling_swf_jiessie/image3.png" alt="截图" class="img-rounded">

这个组件内部为4个按钮，命名分别为：_Btn_1、_Btn_2、_Btn_3、_Btn_4

按钮是一个mc，导出的也是mc

<img src="/assets/images/starling_swf_jiessie/image4.png" alt="截图" class="img-rounded">

我们可以使用JISCuttingButtonGroup进行管理我们的btn列表，如下代码：

	public var _BtnList:JISCuttingButtonGroup;
	public function JISMainUIWindow()
	{
		_BtnList = new JISCuttingButtonGroup("_Btn_",JISButton);
		super("spr_TestWindow", "test");
	setAssetSource(File.applicationDirectory.resolvePath("assets/ui/test/"));
	}

`_BtnList`必须为public类型，并且必须在setAssetSource之前实例化

JISCuttingButtonGroup的第一个参数为需要管理的组件的共有特征命名，第二个参数是设置组件有哪个类进行管理，初始化的时候会new该类，并将按钮显示对象通过setCurrDisplay传入进去。

进行管理之前我们的btn是不断闪烁的，管理之后我们的btn会有一个为选中状态，其他为正常状态

管理window中的对象：

我们的示例文件中有一个空的影片剪辑和一个按钮，如图：

<img src="/assets/images/starling_swf_jiessie/image5.png" alt="截图" class="img-rounded">

由于我们的_ScrollTable是Sprite类型，为系统组件，所以不用提前new出来，我们的_AddTableCellBtn是自定义组件，所以需要事先new出来，代码如下：

	public var _ScrollTable:Sprite;
	public var _AddTableCellBtn:JISButton;
	public function JISMainUIWindow()
	{
		_AddTableCellBtn = new JISButton();
		super("spr_TestWindow", "test");
		setAssetSource(File.applicationDirectory.resolvePath("assets/ui/test/"));
	}
	现在我们创建一个JISScrollTable添加到_ScrollTable中，
	
	/** init会在资源加载完毕并且建立引用之后调用 */
	protected override function init():void
	{
		tableScroll = new JISScrollTable();
		_ScrollTable.addChild(tableScroll);
		tableScroll.getTable().setCellInstanceClass(JISTableTestCell);//设置Table创建个显示对象类型
		tableScroll.getTable().setPreferredWidth(100);//设置Table最大宽度
		tableScroll.getTable().setPreferredCellWidth(48);//设置每个格子宽度，默认为JISTableTestCell#getDisplay()的宽度
		tableScroll.getTable().setPreferredCellHeight(35);//设置每个格子高度，默认为JISTableTestCell#getDisplay()的高度
		//tableScroll.getTable().setIsRow(true);//设置表格布局方式，true横向布局,false竖向布局
		var cellList:Array = [];
		for(;btnIndex<100;btnIndex++) cellList.push(btnIndex);
		tableScroll.width = 100;//设置滚动区域的宽度
		tableScroll.height = 200;//设置滚动区域的高度
		tableScroll.getTable().setCellDatas(cellList,this.getSourceSwf());//创建格子内容
		//按钮点击事件					_AddTableCellBtn.addEventListener(JISButton.BOTTON_CLICK,onBtnClickHandler);
	}
	private function onBtnClickHandler(e:Event):void
	{
		tableScroll.getTable().addCellData(btnIndex++,this.getSourceSwf());//添加新的格子
	}
	JISTableTestCell的代码：
	package
	{
		import jis.ui.JISUISprite;
		import jis.ui.component.JISITableCell;
		
		import lzm.starling.swf.Swf;
		
		import starling.display.DisplayObject;
		import starling.text.TextField;
		
		
		/**
		 * 
		 * @author jiessie 2013-11-27
		 */
		public class JISTableTestCell extends JISUISprite implements JISITableCell
		{
			public var _Text:TextField;
			public function JISTableTestCell(swf:Swf)
			{
				super("","");
				setCurrDisplay(swf.createSprite("spr_TableCell"));
			}
			
			public function setValue(value:*):void
			{
				_Text.text = value;
			}
			
			public override function getDisplay():DisplayObject
			{
				return this;
			}
			
			public function selected(select:Boolean=false):void
			{
			}
			
			public function getValue():*
			{
				return _Text.text;
			}
			
			public override function dispose():void
			{
			}
		}
	}

我们可以看到JISTableTestCell中的构造函数需要一个Swf对象，而创建格子的参数中

tableScroll.getTable().setCellDatas(cellList,this.getSourceSwf());//创建格子内容

tableScroll.getTable().addCellData(btnIndex++,this.getSourceSwf());//添加新的格子

都会传入一个Swf对象，这就可以看出setCellDatas和addCellData会在创建JISTableTestCell的时候将swf传入

JISTableTestCell继承JISUISprite说明JISTableTestCell可以使用该扩展进行管理UI，其中的setCurrDisplay(swf.createSprite("spr_TableCell"));为从资源库中创建一个sprite赋值给当前类，然后当前类会进行管理其中的内容，我们可以看下资源：

<img src="/assets/images/starling_swf_jiessie/image6.png" alt="截图" class="img-rounded">

而我们的JISTableTestCell中也有个_Text属性。

JISTableTestCell中的setValue()可以输出通过setCellDatas和addCellData中设置的值。如此一个支持触控滑动的滚动界面就完毕了，跟手机上绚丽的滑动效果一样哦。

自定义组件管理：

如我们的演示资源中有个进度条面板，其中有_ProgressText、_RowProgress、_AddBtn、_DecBtn

其中_RowProgress是一个mc，其中存在一个背景和一个前景，前景我们命名为_Center，因为扩展提供的JISUIProgressManager需要进度条的名字为_Center，如图：

<img src="/assets/images/starling_swf_jiessie/image7.png" alt="截图" class="img-rounded">

该组件在window中的名字为_ProgressTest，如图：

<img src="/assets/images/starling_swf_jiessie/image8.png" alt="截图" class="img-rounded">

首先我们先建立对应_ProgressTest的类，

	package
	{
		import jis.ui.JISUIManager;
		import jis.ui.component.JISButton;
		import jis.ui.component.JISUIProgressManager;
		
		import starling.events.Event;
		import starling.text.TextField;
		
		
		/**
		 * 
		 * @author jiessie 2013-11-27
		 */
		public class JISProgressTestUIManager extends JISUIManager
		{
			public var _RowProgress:JISUIProgressManager;
			public var _AddBtn:JISButton;
			public var _DecBtn:JISButton;
			public var _ProgressText:TextField;
			
			private var currProgress:int = 0;
			
			public function JISProgressTestUIManager()
			{
				_RowProgress = new JISUIProgressManager();
				_AddBtn = new JISButton();
				_DecBtn = new JISButton();
				super();
			}
			
			protected override function init():void
			{
				_AddBtn.addEventListener(JISButton.BOTTON_CLICK,onClickAddHandler);
				_DecBtn.addEventListener(JISButton.BOTTON_CLICK,onClickDecHandler);
				updateProgress();
			}
			
			private function onClickAddHandler(e:Event):void
			{
				if(currProgress < 100)
				{
					currProgress ++;
					updateProgress();
				}
			}
			
			private function onClickDecHandler(e:Event):void
			{
				if(currProgress > 0)
				{
					currProgress --;
					updateProgress();
				}
			}
			
			private function updateProgress():void
			{
				_RowProgress.setProgress(currProgress,100);
				_ProgressText.text = currProgress+"/100";
			}
		}
	}
	
你可以看到JISProgressTestUIManager是继承的JISUIManager，代表他只是管理UI，不会进行addChild操作

JISProgressTestUIManager中的成员与mc中的成员命名一模一样，层级结构也很类似，只不过一个是代码的层级结构(对象包含属性)，一个是显示对象的层级结构(mc包含mc、TextField)

遇到问题或者更多意见欢迎加QQ群：168436154讨论












