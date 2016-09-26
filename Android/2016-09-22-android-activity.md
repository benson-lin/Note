# Activity

## 创建项目

首先，你需要再新建一个 Android 项目，项目名可以叫做ActivityTest，包名我们就使用默认值 com.example.activitytest。我们不勾选 Create Activity 这个选项，因为这次我们准备手动创建活动，


目前 ActivityTest 项目的 src 目录应该是空的，你应该在 src 目录下先添加一个包。点击Eclipse 导航栏中的 File→New→Package，在弹出窗口中填入我们新建项目时使用的默认包名com.example.activitytest，点击 Finish。

## 编写基本代码

现在右击 com.example.activitytest 包→New→Class，会弹出新建类的对话框，我们新建一个名为 FirstActivity 的类，并让它继承自 Activity，点击 Finish 完成创建。你需要知道，项目中的任何活动都应该重写 Activity 的 onCreate()方法，但目前我们的FirstActivity内部还什么代码都没有，所以首先你要做的就是在 FirstActivity中重写 onCreate()方法

```java
public class FirstActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
}
```
onCreate()方法非常简单，就是调用了父类的 onCreate()方法。当然这只是默认的实现，后面我们还需要在里面加入很多自己的逻辑

## 创建和加载布局

Android 程序的设计讲究逻辑和视图分离，最好每一个活动都能对应一个布局，布局就是用来显示界面内容的，因此我们现在就来手动创建一个布局文件。右击 res/layout 目录→New→Android XML File，会弹出创建布局文件的窗口。我们给这个布局文件命名为 first_layout，根元素就默认选择为 LinearLayout


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/button_1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button 1"
        />
</LinearLayout>

```
android:id 是给当前的元素定义一个唯一标识符，之后可以在代码中对这个元素进行操作。 你可能会对@+id/button_1 这种语法感到陌生，但如果把加号去掉，变成@id/button_1，这你就会觉得有些熟悉了吧，这不就是在 XML 中引用资源的语法吗，只不过是把 string 替换成了 id。是的，如果你需要在 XML 中引用一个 id，就使用@id/id_name 这种语法，而如果你需要在 XML 中定义一个 id，则要使用@+id/id_name 这种语法



接着在 Activity中加载布局

```java
public class FirstActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.first_layout);
    }
}


```


## 在 AndroidManifest 文件中注册

创建项目时文件是这样的
```java
    <application android:allowBackup="true" android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher" android:theme="@style/AppTheme">

    </application>
```

所有的活动都要在 AndroidManifest.xml 中进行注册才能生效

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.activitytest">

    <application android:allowBackup="true" android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher" android:theme="@style/AppTheme">

        <activity
            android:name=".FirstActivity"
            android:label="This is FirstActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
         </activity>

    </application>

</manifest>
```

活动的注册声明要放在<application>标签内，这里是通过<activity>标签来对活动进行注册的。

首先我们要使用 android:name 来指定具体注册哪一个活动。填的.FirstActivity 是什么意思呢？其实这不过就com.example.activitytest.FirstActivity 的缩写而已。由于最外层的 `<manifest>`标签中已经通过 package 属性指定了程序的包名是com.example.activitytest，因此在注册活动时这一部分就可以省略了，直接使用.FirstActivity就足够了。

然后我们使用了 android:label 指定活动中标题栏的内容，标题栏是显示在活动最顶部的，需要注意的是，给主活动指定的 label 不仅会成为标题栏中的内容，还会成为启动器（ Launcher）中应用程序显示的名称。

之后在`<activity>`标签的内部我们加入了`<intent-filter>`标签，并在这个标签里添加了`<action android:name="android.intent.action.MAIN" />`和`<category android:name="android.intent.category.LAUNCHER" />`这两句声明。如果你想让 FirstActivity 作为我们这个程序的主活动，即点击桌面应用程序图标时首先打开的就是这个活动，那就一定要加入这两句声明。

另外需要注意，如果你的应用程序中没有声明任何一个活动作为主活动，这个程序仍然是可以正常安装的，只是你无法在启动器中看到或者打开这个程序。这种程序一般都是作为第三方服务供其他的应用在内部进行调用的，如支付宝快捷支付服务。

然后就可以跑了

## 隐藏标题栏

标题栏中可以进行的操作其实还是蛮多的，尤其是在 Android 4.0 之后加入了 Action Bar的功能。不过有些人会觉得标题栏相当占用屏幕空间，使得内容区域变小，因此也有不少的应用程序会选择将标题栏隐藏掉

只需要加入`requestWindowFeature(Window.FEATURE_NO_TITLE);`； `requestWindowFeature(Window.FEATURE_NO_TITLE)`的意思就是不在活动中显示标题栏，注意这句代码一定要在 setContentView()之前执行，不然会报错。

```java
public class FirstActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.first_layout);
    }
}
```

## 活动中使用Toast


Toast 是 Android 系统提供的一种非常好的提醒方式，在程序中可以使用它将一些短小的信息通知给用户，这些信息会在一段时间后自动消失，并且不会占用任何屏幕空间；


```java
public class FirstActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.first_layout);

        Button button1 = (Button)findViewById(R.id.button_1);
        button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(FirstActivity.this, "You clicked Button 1", Toast.LENGTH_SHORT).show();
            }
        });

    }
}
```

在活动中，可以通过 findViewById()方法获取到在布局文件中定义的元素，这里我们传入 R.id.button_1，来得到按钮的实例，这个值是刚才在 first_layout.xml 中通过 android:id 属性指定的。 findViewById()方法返回的是一个 View 对象，我们需要向下转型将它转成 Button对象。得到了按钮的实例之后，我们通过调用 setOnClickListener()方法为按钮注册一个监听器，点击按钮时就会执行监听器中的 onClick()方法。因此，弹出 Toast 的功能当然是要在onClick()方法中编写了。

Toast 的用法非常简单，通过静态方法 makeText()创建出一个 Toast 对象，然后调用 show()将 Toast 显示出来就可以了。这里需要注意的是， makeText()方法需要传入三个参数。第一个参数是 Context，也就是 Toast 要求的上下文，由于活动本身就是一个 Context 对象，因此这里直接传入 FirstActivity.this即可。第二个参数是 Toast显示的文本内容，第三个参数是 Toast 显示的时长，有两个内置常量可以选择 Toast.LENGTH_SHORT 和 Toast.LENGTH_LONG


## 在活动中使用 Menu

Menu主要用在菜单键中。

需要注意的是：菜单键目前正被谷歌慢慢淘汰。目前很多手机都是用虚拟按键，返回键、Home键、多任务键的三键设计也成为了默认的标准，菜单键也慢慢的被淡化了。要使用菜单键怎么办呢？谷歌给出了解决方案，在需要菜单键的软件界面，系统会自动在右下角出现三个点的按键，用以代替菜单键的功能。比如目前的QQ，在虚拟键盘的右方出现，用来进行版本更新和退出QQ等简单的功能。

那实体按键机呢？其实也不容乐观。不仅如此，现在越来越来越多的实体按键机，它们将菜单键直接改成了多任务键，这画面也毫无违和感。



如果是IDE帮我们创建项目的的时候，默认的Activity中会自动创建了一个 onCreateOptionsMenu()方法。这个方法是用于在活动中创建菜单的；

手机毕竟和电脑不同，它的屏幕空间非常有限，因此充分地利用屏幕空间在手机界面设计中就显得非常重要了。Android 给我们提供了一种方式，可以让菜单都能得到展示的同时，还能不占用任何屏幕的空间

这次我们自己写。

首先在 res 目录下新建一个 menu 文件夹，右击 res 目录→New→Folder，输入文件夹名menu，点击 Finish。接着在这个文件夹下再新建一个名叫 main 的菜单文件，右击 menu 文件
夹→New→Android XML File

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

</menu>
```

加入两个item
```java
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/add_item"
        android:title="Add"/>
    <item
        android:id="@+id/remove_item"
        android:title="Remove" />
</menu>
```

这里我们创建了两个菜单项，其中<item>标签就是用来创建具体的某一个菜单项，然后通过 android:id给这个菜单项指定一个唯一标识符，通过 android:title给这个菜单项指定一个名称。
然后打开 FirstActivity，重写 onCreateOptionsMenu()方法

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    
    getMenuInflater().inflate(R.menu.main, menu);
    return true;
}
```
通过 getMenuInflater()方法能够得到 MenuInflater 对象，再调用它的 inflate()方法就可以给当前活动创建菜单了。 inflate()方法接收两个参数，第一个参数用于指定我们通过哪一个资源文件来创建菜单，这里当然传入 R.menu.main，第二个参数用于指定我们的菜单项将添加到哪一个 Menu 对象当中，这里直接使用 onCreateOptionsMenu()方法中传入的 menu 参数。然后给这个方法返回 true，表示允许创建的菜单显示出来，如果返回了 false，创建的菜单将无法显示。

当然，仅仅让菜单显示出来是不够的，我们定义菜单不仅是为了看的，关键是要菜单真正可用才行，因此还要再定义菜单响应事件。在 FirstActivity中重写 onOptionsItemSelected()方法

```java
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {

        switch (item.getItemId()){

            case R.id.add_item: Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show();
            case R.id.remove_item: Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show();
            default:
        }
        return true;
    }
```

然后启动程序就可以看到了。

## 销毁一个活动

只要按一下 Back 键就可以销毁当前的活动了。不过如果你不想通过按键的方式，而是希望在程序中通过代码来销毁活动，当然也可以， Activity 类提供了一个 finish()方法，我们在活动中调用一下这个方法就可以销毁当前活动了。修改按钮监听器中的代码，如下所示：

```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
	finish();
}
});
```

重新运行程序，这时点击一下按钮，当前的活动就被成功销毁了，效果和按下 Back 键是一样的