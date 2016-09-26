# Intent使用

## 使用显式 Intent

像第二个Activity传递信息

我们现在快速地在 ActivityTest 项目中再创建一个活动。 新建一个 second_layout.xml 布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/button_2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button 2"
        />
</LinearLayout>
```

我们还是定义了一个按钮，按钮上显示 Button 2。然后新建活动 SecondActivity 继承自Activity

```java
public class SecondActivity extends Activity{

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.second_layout);
        Button button1 = (Button)findViewById(R.id.button_2);
        button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(SecondActivity.this, "You clicked Button 2", Toast.LENGTH_SHORT).show();
            }
        });

    }

}
```

然后别忘了注册

```xml
<activity android:name=".SecondActivity">
</activity>
```

由于 SecondActivity 不是主活动，因此不需要配置`<intent-filter>`标签里的内容，注册活动的代码也是简单了许多。 现在第二个活动已经创建完成，剩下的问题就是如何去启动这第二个活动了

Intent 是 Android 程序中各组件之间进行交互的一种重要方式，它不仅可以指明当前组件想要执行的动作，还可以在不同组件之间传递数据。 Intent 一般可被用于**启动活动、启动服务、以及发送广播**等场景。

Intent 的用法大致可以分为两种，显式 Intent 和隐式 Intent


Intent 有多个构造函数的重载，其中一个是 Intent(Context packageContext, Class<?> cls)。这个构造函数接收两个参数，第一个参数 Context 要求提供一个启动活动的上下文，第二个参数 Class 则是指定想要启动的目标活动， 通过这个构造函数就可以构建出 Intent 的“意图”。然后我们应该怎么使用这个 Intent 呢？ Activity 类中提供了一个 startActivity()方法，这个方法是专门用于启动活动的，它接收一个 Intent参数，这里我们将构建好的 Intent传入 startActivity()方法就可以启动目标活动了。


修改 FirstActivity 中按钮的点击事件

```java
        Button button1 = (Button)findViewById(R.id.button_1);
        button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
                startActivity(intent);
            }
        });
```

我们首先构建出了一个 Intent，传入 FirstActivity.this 作为上下文，传入 SecondActivity.class 作为目标活动，这样我们的“意图”就非常明显了，即在 FirstActivity 这个活动的基础上打开 SecondActivity 这个活动。然后通过 startActivity()方法来执行这个 Intent。

可以看到，我们已经成功启动 SecondActivity 这个活动了。如果你想要回到上一个活动怎么办呢？ 很简单，按下 Back 键就可以销毁当前活动，从而回到上一个活动了。


## 使用隐式 Intent

相比于显式 Intent，隐式 Intent 则含蓄了许多，它并不明确指出我们想要启动哪一个活动，而是指定了一系列更为抽象的 action 和 category 等信息替换掉Java中明确指示跳转的代码，然后交由系统去分析这个 Intent，并帮我们找出合适的活动去启动

什么叫做合适的活动呢？ 简单来说就是可以响应我们这个隐式 Intent 的活动

通过在`<activity>`标签下配置`<intent-filter>`的内容， 可以指定当前活动能够响应的 action 和 category，打开 AndroidManifest.xml，添加如下代码：

```xml
<activity android:name=".SecondActivity">
    <intent-filter>
        <action android:name="com.example.activitytest.ACTION_START"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="com.example.activitytest.MY_CATEGORY"/>
    </intent-filter>

 </activity>
```

在`<action>`标签中我们指明了当前活动可以响应com.example.activitytest.ACTION_START 这个 action，而`<category>`标签则包含了一些附加信息，更精确地指明了当前的活动能够响应的 Intent 中还可能带有的 category。只有`<action>`和`<category>`中的内容同时能够匹配上 Intent 中指定的 action 和 category 时，这个活动才能响应该 Intent。

对应的Java代码要怎么写呢？下面的`new Intent("com.example.activitytest.ACTION_START");`其实就是通过category和action的配置，省去了上下文的设置

```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent intent = new Intent("com.example.activitytest.ACTION_START");
		startActivity(intent);
	}
});
```

android.intent.category.DEFAULT 是一种默认的 category，在调用 startActivity()方法的时候如果没有指定特定的 category, 则会自动将这个 category 添加到 Intent 中，下面的代码中就是指定了category。

重新运行程序， 在 FirstActivity 的界面点击一下按钮， 你同样成功启动 SecondActivity
了。不同的是，这次你是使用了隐式 Intent 的方式来启动的，说明我们在`<activity>`标签下配置的 action 和 category 的内容已经生效了！

同时可以看到，每个 Intent 中只能指定一个 action，但却能指定多个 category。目前我们的 Intent 中只有一个默认的 category。

```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent intent = new Intent("com.example.activitytest.ACTION_START");
		intent.addCategory("com.example.activitytest.MY_CATEGORY");
		startActivity(intent);
	}
});
```

## 更多隐式 Intent 的用法

使用隐式 Intent，我们不仅可以启动自己程序内的活动，**还可以启动其他程序的活动**，这使得 Android 多个应用程序之间的功能共享成为了可能

比如说你的应用程序中需要展示一个网页，这时你没有必要自己去实现一个浏览器（事实上也不太可能），而是只需要调用系统的浏览器来打开这个网页就行了。


修改 FirstActivity 中按钮点击事件的代码

```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent intent = new Intent(Intent.ACTION_VIEW);
		intent.setData(Uri.parse("http://www.baidu.com"));
		startActivity(intent);
	}
});
```

这里我们首先指定了 Intent 的 action 是 Intent.ACTION_VIEW，这是一个 Android 系统内置的动作，其常量值为 android.intent.action.VIEW。然后通过 Uri.parse()方法，将一个网址字符串解析成一个 Uri 对象，再调用 Intent 的 setData()方法将这个 Uri 对象传递进去。重新运行程序，在 FirstActivity 界面点击按钮就可以看到打开了系统浏览器。


setData()部分接收一个 Uri 对象，主要用于指定当前 Intent 正在操作的数据，而这些数据通常都是以字符串的形式传入到 Uri.parse()方法中解析产生的。


与此对应，我们还可以在<intent-filter>标签中再配置一个`<data>`标签，用于更精确地指定当前活动能够响应什么类型的数据。 `<data>`标签中主要可以配置以下内容。


1. android:scheme
用于指定数据的协议部分，如上例中的 http 部分。
2. android:host
用于指定数据的主机名部分，如上例中的 www.baidu.com 部分。
3. android:port
用于指定数据的端口部分，一般紧随在主机名之后。
4. android:path
用于指定主机名和端口之后的部分，如一段网址中跟在域名之后的内容。
5. android:mimeType
用于指定可以处理的数据类型，允许使用通配符的方式进行指定。

只有`<data>`标签中指定的内容和 Intent 中携带的 Data 完全一致时，当前活动才能够响应
该 Intent。不过一般在`<data>`标签中都不会指定过多的内容，如上面浏览器示例中，其实只
需要指定 android:scheme 为 http，就可以响应所有的 http 协议的 Intent 了。


新建 third_layout.xml 布局文件
```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/button_3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button 3"
        />
</LinearLayout>

```

然后新建活动 ThirdActivity 继承自 Activity

```java
public class ThirdActivity extends Activity{

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.third_layout);
    }
}
```
最后在 AndroidManifest.xml 中为 ThirdActivity 进行注册

```xml
        <activity android:name=".ThirdActivity">
           <intent-filter>
               <action android:name="android.intent.action.VIEW"/>
               <category android:name="android.intent.category.DEFAULT"/>
               <data android:scheme="http" />
           </intent-filter>
        </activity>
```


我 们 在 ThirdActivity 的 `<intent-filter>` 中 配 置 了 当 前 活 动 能 够 响 应 的 action 是 Intent.ACTION_VIEW 的常量值，而 category 则毫无疑问指定了默认的 category 值，另外在`<data>`标签中我们通过 android:scheme 指定了数据的协议必须是 http 协议，这样 ThirdActivity**应该就和浏览器一样，能够响应一个打开网页的 Intent 了**。(对某些手机会出现总是打开系统自带浏览器的情况，此时需要将默认浏览器禁用)


在运行程序，点击Button1，会出现除了默认浏览器，还会有一个选项是我们的应用程序，因为我们的应用程序也设置了这个Intent

点击 Browser 还会像之前一样打开浏览器，并显示百度的主页，而如果点击了 ActivityTest，则会启动 ThirdActivity。需要注意的是，虽然我们声明了 ThirdActivity 是可以响应打开网页的Intent 的，但实际上这个活动并没有加载并显示网页的功能，所以在真正的项目中尽量不要去做这种有可能误导用户的行为，不然会让用户对我们的应用产生负面的印象。

除了 http 协议外，我们还可以指定很多其他协议，比如 geo 表示显示地理位置、 tel 表示拨打电话。下面的代码展示了如何在我们的程序中调用系统拨号界面。


```java
   button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                Intent intent = new Intent(Intent.ACTION_VIEW);
                intent.setData(Uri.parse("tel:10086"));
                startActivity(intent);
            }
        });
```

首先指定了 Intent 的 action 是 Intent.ACTION_DIAL，这又是一个 Android 系统的内置动作。然后在 data 部分指定了协议是 tel，号码是 10086。


## 向下一个活动传递数据

到目前为止，我们都只是简单地使用 Intent 来启动一个活动，其实 Intent 还可以在启动活动的时候传递数据的

在启动活动时传递数据的思路很简单， Intent 中提供了一系列 putExtra()方法的重载，可以把我们想要传递的数据暂存在 Intent 中，启动了另一个活动后，只需要把这些数据再从Intent 中取出就可以了。比如说 FirstActivity 中有一个字符串，现在想把这个字符串传递到SecondActivity 中


```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		String data = "Hello SecondActivity";
		Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
		intent.putExtra("extra_data", data);
		startActivity(intent);
	}
});
```

这里我们还是使用显式 Intent 的方式来启动 SecondActivity，并通过 putExtra()方法传递了一个字符串。注意这里 putExtra()方法接收两个参数，第一个参数是键，用于后面从 Intent中取值，第二个参数才是真正要传递的数据。


然后我们在 SecondActivity 中将传递的数据取出

```java

public class SecondActivity extends Activity {
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.second_layout);
		Intent intent = getIntent();
		String data = intent.getStringExtra("extra_data");
		Log.d("SecondActivity", data);
	}
}
```

首先可以通过 getIntent()方法获取到用于启动 SecondActivity 的 Intent，然后调用getStringExtra()方法，传入相应的键值，就可以得到传递的数据了。这里由于我们传递的是字符串，所以使用 getStringExtra()方法来获取传递的数据，如果传递的是整型数据，则使用getIntExtra()方法，传递的是布尔型数据，则使用 getBooleanExtra()方法，以此类推。


## 返回数据给上一个活动

返回上一个活动只需要按一下 Back 键就可以了， 并没有一个用于启动活动 Intent 来传递数据。通过查阅文档你会发现， Activity 中还有一个 startActivityForResult()方法也是用于启动活动的，但这个方法期望在活动销毁的时候能够返回一个结果给上一个活动。毫无疑问，这就是我们所需要的

startActivityForResult()方法接收两个参数，第一个参数还是 Intent，第二个参数是请求
码，用于在之后的回调中判断数据的来源。我们还是来实战一下， 修改 FirstActivity 中按钮
的点击事件，代码如下所示：

```java
button1.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
		startActivityForResult(intent, 1);
	}
});
```

这里我们使用了 startActivityForResult()方法来启动 SecondActivity，请求码只要是一个唯一值就可以了，这里传入了 1。接下来我们在 SecondActivity 中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑

```java
button2.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent intent = new Intent();
		intent.putExtra("data_return", "Hello FirstActivity");
		setResult(RESULT_OK, intent);
		finish();
	}
});
```

可以看到，我们还是构建了一个 Intent，只不过这个 Intent 仅仅是用于传递数据而已，它没有指定任何的“意图”。紧接着把要传递的数据存放在 Intent 中，然后调用了 setResult()方法。这个方法非常重要，是专门用于向上一个活动返回数据的。 setResult()方法接收两个参数，第一个参数用于向上一个活动返回处理结果，一般只使用 RESULT_OK 或RESULT_CANCELED 这两个值，第二个参数则是把带有数据的 Intent 传递回去， 然后调用了 finish()方法来销毁当前活动。


由于我们是使用 startActivityForResult()方法来启动 SecondActivity 的，在 SecondActivity
被销毁之后会回调上一个活动的 onActivityResult()方法，因此我们需要在 FirstActivity 中重
写这个方法来得到返回的数据，

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
	switch (requestCode) {
		case 1:
			if (resultCode == RESULT_OK) {
				String returnedData = data.getStringExtra("data_return");
				Log.d("FirstActivity", returnedData);
			}
			break;
		default:
	}
}
```

onActivityResult()方法带有三个参数，第一个参数 requestCode，即我们在启动活动时传入的请求码。第二个参数 resultCode，即我们在返回数据时传入的处理结果。第三个参数 data，即携带着返回数据的 Intent。由于在一个活动中有可能调用 startActivityForResult()方法去启动很多不同的活动，每一个活动返回的数据都会回调到 onActivityResult()这个方法中，因此我 们 首 先 要 做 的 就 是 通过 检 查 requestCode 的值 来 判 断 数 据 来 源 。 确定 数 据 是 从SecondActivity 返回的之后，我们再通过 resultCode 的值来判断处理结果是否成功。最后从data 中取值并打印出来， 这样就完成了向上一个活动返回数据的工作

这时候你可能会问，如果用户在 SecondActivity 中并不是通过点击按钮，而是通过按下Back 键回到 FirstActivity， 这样数据不就没法返回了吗？没错，不过这种情况还是很好处理的，我们可以通过重写 onBackPressed()方法来解决这个问题

```java
@Override
public void onBackPressed() {
	Intent intent = new Intent();
	intent.putExtra("data_return", "Hello FirstActivity");
	setResult(RESULT_OK, intent);
	finish();
}
```

这样的话，当用户按下 Back 键，就会去执行onBackPressed()方法中的代码，我们在这里添加返回数据的逻辑就行了。