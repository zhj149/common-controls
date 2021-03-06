# CommonControl
为TornadoFx的封装的常用控件与工具,基于Jfoenix,借鉴Kfoenix
<meta name="referrer" content="no-referrer">

<img src="https://jitpack.io/v/Stars-One/common-controls.svg" />

这个开源库原本我也不想开源出来的,毕竟花费了自己很久的时间,但是想到TornadoFx国内实在没有多少人用,便是想开源出来了,国内TornadoFx资料较少,有些组件并没有,只能靠自己来造。

在这个前端为主的时代，可能只有我这种人还在独自坚持研究这种小众的技术吧（至少国内是这样的情况）

**希望你在使用之前可以根据自己的实际情况给予打赏，算是对我的鼓励，你的打赏是我后期维护并积极更新的动力！谢谢**

![](https://img2020.cnblogs.com/blog/1210268/202003/1210268-20200316120825333-1551152974.png)

TornadoFX交流群：1071184701

[TornadoFx中文文档](https://stars-one.gitee.io/tornadofx-guide-chinese) 目前还在翻译中

本库包含了之前的[IconText](https://github.com/Stars-One/IconTextFx)和[DialogBuilder](https://github.com/Stars-One/DialogBuilder)

- **[IconText](https://github.com/Stars-One/IconTextFx)** 5000+个Material Design字体图标库

- **[DialogBuilder](https://github.com/Stars-One/DialogBuilder)** 基于Jfoenix的对话框生成器


最新版本1.7

## 引入依赖

### Maven引入

**1. 添加仓库**

由于jar包是上传在jitpack仓库中,所以得在项目的pom.xml添加仓库
```
<repositories>
	<repository>
		<id>jitpack.io</id>
		<url>https://jitpack.io</url>
	</repository>
</repositories>
```

**2.添加依赖**
```
<dependency>
	<groupId>com.github.Stars-One</groupId>
	<artifactId>common-controls</artifactId>
	<version>1.6</version>
</dependency>
```

### Gradle引入

```
allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}
```

```
dependencies {
	implementation 'com.github.Stars-One:common-controls:1.6'
}
```
## 介绍

注意:考虑到查找的方便,1.6版本之后控件的名字发生了变化,**控件名多个前缀x,且采用了驼峰命名法**,如`jfxbutton`变为了`xJfxButton`**,其他依次类推

控件主要分为以下几个大类:

- 对话框
- 检测更新
- 常用方法
- 常用控件
- 下载框架(另有独立版,不过暂未发布,敬请期待)

## 1.常用对话框
对话框提供了整合了之前的DiglogBuilder,并新增加了加载对话框和自定义对话框内容,参考了Kf项目,我把有些对话框整合成了Kotlin中的DSL方式调用,有些对话框就没有
### 普通对话框

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718194948541-1031130858.png)

```
jfxbutton("测试消息") {
	action {
		jfxdialog(currentStage, "标题", "内容")
	}
}
```
### 确认对话框

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718200034445-114242192.png)

```
jfxbutton("测试确认对话框") {
	action {
		DialogBuilder(currentStage)
				.setTitle(title)
				.setMessage("hello")
				.setNegativeBtn("取消"){ println("点击了取消按钮")}
				.setPositiveBtn("确定") { println("点击了确定按钮")}
				.create()
	}
}
```

PS:提供了改变颜色,在对应的setBtn方法添加颜色即可,颜色支持**十六进制和文字**,如

```
jfxbutton("测试确认对话框") {
	action {
		DialogBuilder(currentStage)
				.setTitle(title)
				.setMessage("hello")
				.setNegativeBtn("取消","red"){ println("点击了取消按钮")}
				.setPositiveBtn("确定","#fafafa") { println("点击了确定按钮")}
				.create()
	}
}
```
### 输入对话框

弹出对话框,让用户输入内容

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718200607812-1708539890.png)

```
jfxbutton("测试输入对话框") {
	action {
		//text即为用户输入的内容
		jfxdialog(currentStage, "带输入框的对话框", "输入整数内容", { text -> println(text) })
	}
}
```

### 输出对话框
此对话框用来提示对应的网址或者是文件的位置,用户点击之后可以跳转浏览器或者是打开资源管理器并定位到文件

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718200353381-2010535738.png)

```
jfxbutton("测试输出对话框") {
	action {
		//文件
		jfxdialog(currentStage, "带链接的输入框", "输出目录为","D:\\text.txt"
				,false)
		//网址
		jfxdialog(currentStage, "带链接的输入框", "输出目录为","stars-one.site"
				,true)
	}
}
```
### 下载对话框

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718202317796-1270345515.png)

```
jfxbutton("下载"){
	action{
		DownloadDialogView(currentStage,"下载地址","D:\\test.txt")
			.show()
	}
}
```

PS:第三个参数路径可不填,则自动截取下载地址的末尾的作为文件名,并下载到与jar的同目录文件中

### 加载对话框
![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718200839449-112125409.png)

```
jfxbutton("测试加载对话框") {
	action {
		loadingDialog(currentStage,"标题","内容"){alert ->
			runAsync {
				//这里做网络请求或者其他耗时的操作
				for (i in 0..3) {
					Thread.sleep(200)
					println(i)
				}
			} ui{
				//调用close或者hideWithAnimation关闭对话框
				alert.hideWithAnimation()
			}

		}

	}
}
```

### 关闭程序对话框

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718204207727-1864873197.png)

```
jfxbutton("关闭程序对话框") {
	action {
		stopDialog(currentStage,"标题","点击确定结束当前程序")
	}
}
```

### 检测更新对话框
这里单独抽出来讲解使用,详见第2部分
### 自定义对话框
若是你不满足已有的对话框,你可以按照你的喜欢,自定义对话框的内容

```
//这里你可以自定义布局
val content = HBox(Text("自定义内容"))
val alert = DialogBuilder(currentStage)
                .setCustomContext(content)
                .setNegativeBtn("确定")
                .create()
```
## 2.检测更新功能

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718205016359-489175058.png)

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718205052584-1083320179.png)

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718205307205-1422140750.png)

![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200718205709613-1098344008.png)

基于上面的对话框,实现了自动检测更新的框架

我这里是使用了博客园和蓝奏云作为更新的平台,博客园用来发布新版本的信息,蓝奏云则用来上传更新文件

使用之前,你需要在博客园发布一篇随笔,并以下面的表格形式,这里给出对应的md格式

```
|版本名|版本号	|更新时间	|更新内容	|更新地址|
|--	|--	|--	|--	|--	|
|v1.1	|2	|2020-7-10	|1.更新xx\n2.更新xx	|	|
|v1.0	|1	|2020-7-10	|1.更新xx\n2.更新xx	|	|
```

更新内容一栏中,通过`\n`来实现换行 

更新地址,你可以使用蓝奏云的地址或者是其他的地址,**如果使用蓝奏云的话,一定要是整个文件夹的地址**

更新版本时,你需要将博客园上随笔的内容进行更新,并在新版本的信息放在表格第一行,并保证版本号比之前的要高,否则框架无法检测到新版本的升级(使用蓝奏云的话,则在之前的文件夹上传文件即可,框架会自动下载第一个的文件,即为最新版本的文件)

```
jfxbutton("检测更新") {
	action {
		//最重要的是版本号,记得是int类型的,版本名无关紧要,只是让之后的升级提示对话框显示可以好看点
		TornadoFxUtil.checkVersion(currentStage, "https://www.cnblogs.com/stars-one/p/13284015.html", "当前版本号", "当前版本名")
	}
}
```
## 3.常用方法
位于TornadoFxUtils.kt文件中

- 下载文件`downloadFile`
- 下载图片`downloadImage`
- 复制文字到粘贴板`copyTextToClipboard`
- 自动补全网址`completeUrl`
- 检测版本更新`checkVersion`
- 打开网址`openUrl`

## 4.常用控件
以下的控件其实本质上都是一个方法,使用了TornadoFx内置的DSL语法进行书写,使用的时候和TornadoFx编写布局的代码是一样的

|控件	|控件名称	|例子|
|--	|--	|--	|
|xUrlLink	|可选择的超链接文本	|urlLink("博客地址","stars-one.site")|
|xImageView	|生成指定宽高的图片,正方形的图片可省略高度参数	|imageview("xx.jpg",50,50)	|
|xIconItem	|生成带图标的右键菜单(ContextMenu),每个控件都有setContextMenu方法	|![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200719114145732-112475743.png)|
|xSelectText|可选择的文本框|selectText("内容")|
|xFfxButton|指定宽高的扁平按钮,正方形可省略高度参数|jfxbutton("xx.jpg",50,50)|
|xCircleJfxbutton|圆形图标扁平按钮(鼠标滑过会有阴影),传递一个node参数|circlejfxbutton(imageview("xx.jpg",50))|
|xFileTextField|文件输入+选择|selectText("内容")|
|showToast|显示Toast|![](https://img2020.cnblogs.com/blog/1210268/202007/1210268-20200719122328168-1367451505.png)
|

## 5.下载框架
对应的HttpDownloader类

构造方法需要传入一个下载地址和保存文件路径,之后调用`startDownload`方法即可下载
```
HttpDownloader(downloadUrl, file).startDownload(object : HttpDownloader.OnDownloading {
	override fun onProgress(progress: Double, percent: Int, speed: String) {
		//回调的几个参数,progress是进度,percent是百分比,speed是下载速度
		//你可以使用在下载之前将相关得到控件绑定一个观察者,之后将这里的相关参数设置即可
		
	}

	override fun onFinish() {
		
	}

	override fun onError(e: IOException) {
		
	}
})
```

## 6.XRecyclerView的用法

仿造Android中的RecyclerView用法写的一个控件,主要实现将一个List数据以列表的方式显示出来

旧版本(及命名为FxRecyclerView)使用方法详见[Tornadofx控件库(2)——FxRecyclerView | Stars-One的杂货小窝](https://stars-one.site/2020/07/31/fxrecyclerview)

**XRecyclerView为使用MVVM模式重构的版本,使用起来更为的方便**

### 1.创建bean类
这个没啥好说的，就是一个存数据的bean类，如一个`Person`类，根据自己的情况与需求创建
```
data class Person(var name: String,var age:String)
```
### 2.创建ItemView
这个就是列表中的每一项View，**需要继承tornadofx中的View**，我这里就是显示Person的name和age属性，比较简单演示一下

**PS:我自己封装了一个抽象类ItemViewBase,可以直接实现此类即可**

```
class MyItemView : ItemViewBase<Person, MyItemView>(null,null){
    var nameText by singleAssign<TextField>()
    var ageText by singleAssign<TextField>()

    /**
     * 设置输入的改变,当用户输入内容时候,同时改变obList中的beanList数据
     *
     */
    override fun inputChange() {
        val name = nameText.text
        val age = ageText.text
        val copy = bean.copy()
        copy.name = name
        copy.age = age
        obList.update(bean, copy)
    }

    /**
     * 初次设置数值,直接设置相关控件对象的数值
     *
     * @param beanT
     */
    override fun bindData(beanT: Person) {
        nameText.text = beanT.name
        ageText.text = beanT.age
    }

    override val root = hbox {
        nameText = textfield() {

        }
        ageText = textfield {
        }

        //这里必须要调用此方法,将当前所有输入控件传入,进行设置数值变化监听
        setDataChange(nameText,ageText)
    }
}
```

### 3.创建RvDataObservableList和RvAdapter

RvDataObservableList里面包含了beanList和itemViewList,**后续只需要调用其相关的方法即可实现对列表数据进行的增删改查,且会同步itemVIew进行修改(即界面会同步根据数据改变)**

**PS:建议把此类RvDataObservableList声明为全局变量**

```
//Person是bean的实体类,MyItemView则是itemview的类
val dataList = RvDataObservableList<Person,MyItemView>()

val adapter = object : RvAdapter<Person, MyItemView>(dataList) {

    override fun onBindData(itemView: MyItemView, bean: Person, position: Int) {
        //这里bindData是ItemViewBase中定义的方法
        itemView.bindData(dataList,position)
    }

    //itemView的单击事件
    override fun onClick(itemView: MyItemView, position: Int) {
        println("单击$position")
    }

    //itemView的右击事件
    override fun onRightClick(itemView: MyItemView, position: Int) {
        println("右击$position")
    }

    override fun onCreateView(): MyItemView {
        return MyItemView()
    }

}
```

### 4.界面添加FxRecyclerView
```
class RvTestView : View("My View") {
    val dataList = RvDataObservableList<Person,MyItemView>()

    override val root = vbox {
        setPrefSize(1000.0, 400.0)       

        val adapter = object : RvAdapter<Person, MyItemView>(dataList) {

            override fun onBindData(itemView: MyItemView, bean: Person, position: Int) {
                itemView.bindData(dataList,position)
            }

            override fun onClick(itemView: MyItemView, position: Int) {
                println("单击$position")
            }

            override fun onRightClick(itemView: MyItemView, position: Int) {
                println("右击$position")
            }

            override fun onCreateView(): MyItemView {
                return MyItemView()
            }
        }

        this+=XRecyclerView<Person, MyItemView>().setRvAdapter(adapter)
    }
}
```

### 5.方法补充

**XRecyclerView:**

|方法名							|参数说明	|方法说明				|
|--								|--			|--						|
|setWidth(double)				|double类型的数值			|设置宽度				|
|setHegiht(double)				|double类型的数值			|设置高度				|
|setIsShowHorizontalBar(String)	|显示方式，never(不显示） always（一直显示） asneed（自动根据需要显示）			|设置是否显示水平滚动条	|

**RvDataObservableList:**

**注意:下面涉及到对beanList的修改,都会同步对界面进行修改**

|方法名							|参数说明	|方法说明				|
|--								|--			|--						|
|getItemView(bean: beanT)			|bean对象		|根据bean对象获取itemView				|
|getItemView(index: Int)				|double类型的数值			|设置高度				|
|add(beanT)						|			|添加一个数据			|
|add(index,beanT)						|			|指定坐标插入新数据		|
|addAll(list: List<beanT>)						|			|添加多个新数据		|
|addAll(vararg bean: beanT)					|			|添加多个新数据	|
|update(index: Int, bean: beanT)				|			|更新指定坐标数据		|
|update(bean: beanT,newBean:beanT)				|			|把某个对象更新为新对象newBean		|
|remove(bean: beanT)				|			|指定坐标插入新数据		|
|remove(index: Int)				|			|移除指定下标		|
|remove(vararg indexList:Int)			|			|移除指定的多个下标		|
|remove(indexList: List<Int>)			|			|移除指定的多个下标		|
|fun remove(from: Int, to: Int)			|			|移除连续的多个指定下标( 区间为[from,to) )		|
|removeLast()				|			|移除最后一个数据		|
|clear				|			|清除所有数据		|


