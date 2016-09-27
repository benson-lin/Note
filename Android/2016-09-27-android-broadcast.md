# 广播机制

## 简介


因为 Android 中的每个应用程序都可以对自己感兴趣的广播进行注册，这样该程序就只会接收到自己所关心的广播内容，这些广播可能是来自于系统的，也可能是来自于其他应用程序的。 Android 提供了一套完整的 API，允许应用程序自由地发送和接收广播。**发送广播的方法就是借助 Intent**。而接收广播的方法则需要引入一个新的概念，**广播接收器**（ Broadcast Receiver）。


Android 中的广播主要可以分为两种类型，**标准广播和有序广播**。

**标准广播**（ Normal broadcasts） 是一种**完全异步执行**的广播，在广播发出之后，所有的广播接收器几乎都会在同一时刻接收到这条广播消息，因此它们之间没有任何先后顺序可言。这种广播的效率会比较高，但同时也意味着它是无法被截断的。

**有序广播**（ Ordered broadcasts） 则是一种**同步执行**的广播，在广播发出之后，同一时刻只会有一个广播接收器能够收到这条广播消息，当这个广播接收器中的逻辑执行完毕后，广播才会继续传递。所以此时的广播接收器是有先后顺序的，优先级高的广播接收器就可以先收到广播消息，并且前面的广播接收器还可以截断正在传递的广播，这样后面的广播接收器就无法收到广播消息了。通过`<intent-filter android:priority="100">`设置。


## 接收系统广播

Android 内置了很多系统级别的广播，我们可以在应用程序中通过监听这些广播来得到各种系统的状态信息。比如手机开机完成后会发出一条广播，电池的电量发生变化会发出一条广播，时间或时区发生改变也会发出一条广播等等。如果想要接收到这些广播，就需要使用广播接收器

### 动态注册监听网络变化

广播接收器可以自由地对自己感兴趣的广播进行注册（IntentFilter），这样当有相应的广播发出时，广播接收器就能够收到该广播，并在内部处理相应的逻辑。注册广播的方式一般有两种，**在代码中注册和在 AndroidManifest.xml 中注册**，其中前者也被称为动态注册，后者也被称为静态注册.

那么该如何创建一个广播接收器呢？其实只需要新建一个类，让它继承自 BroadcastReceiver，并重写父类的 onReceive()方法就行了。这样当有广播到来时， onReceive()方法就会得到执行，具体的逻辑就可以在这个方法中处理。

那我们就先通过动态注册的方式编写一个能够监听网络变化的程序，借此学习一下广播接收器的基本用法吧。新建一个 BroadcastTest 项目，然后修改 MainActivity 中的代码

```java

public class MainActivity extends Activity {
	private IntentFilter intentFilter;
	private NetworkChangeReceiver networkChangeReceiver;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		intentFilter = new IntentFilter();
		intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
		networkChangeReceiver = new NetworkChangeReceiver();
		registerReceiver(networkChangeReceiver, intentFilter);
	}
	@Override
	protected void onDestroy() {
		super.onDestroy();
		unregisterReceiver(networkChangeReceiver);
	}
	class NetworkChangeReceiver extends BroadcastReceiver {
		@Override
		public void onReceive(Context context, Intent intent) {
			Toast.makeText(context, "network changes",
			Toast.LENGTH_SHORT).show();
		}
	}
}


```


可以看到，我们在 MainActivity 中定义了一个内部类 NetworkChangeReceiver，这个类是继承自 BroadcastReceiver 的，并重写了父类的 onReceive()方法。这样每当网络状态发生变化时， onReceive()方法就会得到执行，这里只是简单地使用 Toast 提示了一段文本信息。

然后观察 onCreate()方法，首先我们创建了一个 IntentFilter 的实例，并给它添加了一个值为 android.net.conn.CONNECTIVITY_CHANGE 的 action，为什么要添加这个值呢？因为当网络状态发生变化时，系统发出的正是一条值为 android.net.conn.CONNECTIVITY_CHANGE 的广播，也就是说我们的广播接收器想要监听什么广播，就在这里添加相应的action 就行了。接下来创建了一个 NetworkChangeReceiver 的实例，然后调用 registerReceiver()方法进行注册，将 NetworkChangeReceiver 的实例和 IntentFilter 的实例都传了进去，这样NetworkChangeReceiver 就会收到所有值为 android.net.conn.CONNECTIVITY_CHANGE 的广播，也就实现了监听网络变化的功能。

最后要记得，动态注册的广播接收器一定都**要取消注册**才行，这里我们是在 onDestroy()方法中通过调用 unregisterReceiver()方法来实现的。


运行程序，首先你会在注册完成的时候收到一条广播，然后按下 Home 键回到主界面（注意不能按 Back 键，否则 onDestroy()方法会执行）

不过只是提醒网络发生了变化还不够人性化，最好是能准确地告诉用户当前是有网络还是没有网络，因此我们还需要对上面的代码进行进一步的优化。


```java
    class NetworkChangeReceiver extends BroadcastReceiver{

        @Override
        public void onReceive(Context context, Intent intent) {


            ConnectivityManager connectivityManager = (ConnectivityManager)getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if(networkInfo != null && networkInfo.isAvailable()){
                Toast.makeText(context, "network is available", Toast.LENGTH_SHORT).show();
            }else{
                Toast.makeText(context, "network is unavailable", Toast.LENGTH_SHORT).show();
            }

            Toast.makeText(context, "network changes", Toast.LENGTH_SHORT).show();
        }
    }
```

在onReceive()方法中，首先通过 getSystemService()方法得到了 ConnectivityManager 的实例，这是一个系统服务类，专门用于管理网络连接的。然后调用它的 getActiveNetworkInfo()方法可以得到 NetworkInfo 的实例，接着调用 NetworkInfo 的 isAvailable()方法，就可以判断出当前是否有网络了，最后我们还是通过 Toast 的方式对用户进行提示。这样还不能运行

因为Android 系统为了保证**应用程序的安全性**做了规定，如果程序需要访问一些系统的关键性信息，**必须在配置文件中声明权限**才可以，否则程 序 将会 直 接崩 溃，比 如 这里 查 询系 统的网 络 状态 就 是需 要声明 权 限的 。 打开AndroidManifest.xml 文件，在里面加入如下权限就可以查询系统网络状态了：

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="com.example.broadcasttest"
	android:versionCode="1"
	android:versionName="1.0" >
	<uses-sdk
		android:minSdkVersion="14"
		android:targetSdkVersion="19" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
	……
</manifest>
```

访问 http://developer.android.com/reference/android/Manifest.permission.html可以查看 Android系统所有可声明的权限。

### 静态注册实现开机启动

动态注册的广播接收器可以自由地控制注册与注销，在灵活性方面有很大的优势，但是它也存在着一个缺点，即必须要在程序启动之后才能接收到广播，因为注册的逻辑是写在onCreate()方法中的。那么有没有什么办法可以让程序在未启动的情况下就能接收到广播呢？这就需要使用静态注册的方式了。

这里我们准备让程序接收一条开机广播，当收到这条广播时就可以在 onReceive()方法里执行相应的逻辑，从而**实现开机启动的功能**。新建一个 BootCompleteReceiver 继承自BroadcastReceiver

```java
public class BootCompleteReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {

        Toast.makeText(context, "Boot Complete", Toast.LENGTH_SHORT).show();
    }
}
```
 注册：
```xml
<receiver android:name=".BootCompleteReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

终于， `<application>`标签内出现了一个新的标签`<receiver>`，所有静态注册的广播接收器都是在这里进行注册的。它的用法其实和`<activity>`标签非常相似，首先通过 android:name来指定具体注册哪一个广播接收器，然后在<intent-filter>标签里加入想要接收的广播就行了，由于 Android系统启动完成后会发出一条值为 android.intent.action.BOOT_COMPLETED的广播，因此我们在这里添加了相应的 action。

然后开机启动后就可以收到这个广播了。


## 发送自定义广播

### 发送标准广播


在发送广播之前，我们还是需要先定义一个广播接收器来准备接收此广播才行， 不然发出去也是白发。因此新建一个 MyBroadcastReceiver 继承自 BroadcastReceiver

```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {

        Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
    }
}
```


这里当 MyBroadcastReceiver收到自定义的广播时，就会弹出 received in MyBroadcastReceiver的提示。然后在 AndroidManifest.xml 中对这个广播接收器进行注册

```xml
<receiver android:name=".MyBroadcastReceiver">
    <intent-filter>
        <action android:name="com.zebin.broadcasttest.MY_BROADCAST"/>
    </intent-filter>
</receiver>
```

可以看到，这里让 MyBroadcastReceiver 接收一条值为 com.example.broadcasttest.MY_BROADCAST 的广播，因此待会儿在发送广播的时候，我们就需要发出这样的一条广播。接下来修改 activity_main.xml 中的代码，

```xml
<Button
    android:id="@+id/button"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />

```

这里在布局文件中定义了一个按钮，用于作为发送广播的触发点。然后修改 MainActivity中的代码

```java
button = (Button)findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent intent = new Intent("com.zebin.broadcasttest.MY_BROADCAST");
        sendBroadcast(intent, null);
    }
});
```

可以看到，我们在按钮的点击事件里面加入了发送自定义广播的逻辑。首先构建出了一个 Intent 对象，并把要发送的广播的值传入，然后调用了 Context 的 sendBroadcast()方法将广播发送出去，这样所有监听 com.example.broadcasttest.MY_BROADCAST 这条广播的广播接收器就会收到消息。此时发出去的广播就是一条标准广播。


### 发送有序广播

广播是一种可以跨进程的通信方式，这一点从前面接收系统广播的时候就可以看出来了。因此在我们应用程序内发出的广播，其他的应用程序应该也是可以收到的。为了验证这一点，我们需要再新建一个 BroadcastTest2 项目。

新建 AnotherBroadcastReceiver 继承自 BroadcastReceiver

```java
public class AnotherBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "AnotherBroadcastReceiver receiverd the message", Toast.LENGTH_LONG).show();
    }
}
```

注册

```xml
<receiver android:name=".AnotherBroadcastReceiver">
    <intent-filter>
        <action android:name="com.zebin.broadcasttest.MY_BROADCAST"/>
    </intent-filter>
</receiver>
```

可 以 看 到 ， AnotherBroadcastReceiver 同 样 接 收 的 是com.example.broadcasttest.
MY_BROADCAST 这条广播。

运行程序，点击按钮，发现两个程序都接收到了广播，说明我们的应用程序发出的广播是可以被其他的应用程序接收到的

不过到目前为止，程序里发出的都还是标准广播，现在我们来尝试一下发送有序广播。关闭 BroadcastTest2 项目，然后修改 BroadcastTest 项目都的 MainActivity 中的代码

```java
        button = (Button)findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("com.zebin.broadcasttest.MY_BROADCAST");
                sendOrderedBroadcast(intent, null);
            }
        });
```

可以看到，发送有序广播只需要改动一行代码，即将 sendBroadcast()方法改成sendOrderedBroadcast()方法。 sendOrderedBroadcast()方法接收两个参数，第一个参数仍然是Intent，第二个参数是一个与权限相关的字符串，这里传入 null 就行了。

现在重新运行程序，并点击 Send Broadcast按钮，你会发现，两个应用程序仍然都可以接收到这条广播。看上去好像和标准广播没什么区别嘛，不过别忘了，这个时候的广播接收器是有**先后顺序**的，而且前面的广播接收器还可以**将广播截断**，以阻止其继续传播。

那么该如何设定广播接收器的先后顺序呢？当然是在注册的时候进行设定的了，修改
AndroidManifest.xml 中的代码

```xml
<receiver android:name=".MyBroadcastReceiver">
    <intent-filter android:priority="100">
        <action android:name="com.zebin.broadcasttest.MY_BROADCAST"/>
    </intent-filter>
</receiver>
```

可以看到，我们通过 android:priority属性给广播接收器设置了优先级，优先级比较高的广播接收器就可以先收到广播。这里将 MyBroadcastReceiver 的优先级设成了 100，以保证它一定会在 AnotherBroadcastReceiver 之前收到广播。

既然已经获得了接收广播的优先权，那么 MyBroadcastReceiver 就可以选择是否允许广播继续传递了。修改 MyBroadcastReceiver 中的代码

```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {

        Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
        abortBroadcast();
    }
}

```
如果在 onReceive()方法中调用了 abortBroadcast()方法，就表示将这条广播截断，后面的广播接收器将无法再接收到这条广播。现在重新运行程序，并点击一下 Send Broadcast 按钮，你会发现，只有 MyBroadcastReceiver 中的 Toast 信息能够弹出，说明这条广播经过MyBroadcastReceiver 之后确实是终止传递了


## 使用本地广播

前面我们发送和接收的广播全部都是属于系统全局广播，即发出的广播可以被其他任何的任何应用程序接收到，并且我们也可以接收来自于其他任何应用程序的广播。这样就很容易会引起安全性的问题，比如说我们发送的一些携带关键性数据的广播有可能被其他的应用程序截获，或者其他的程序不停地向我们的广播接收器里发送各种垃圾广播。

为了能够简单地解决广播的安全性问题， Android 引入了一套本地广播机制，使用这个机制发出的广播只能够在应用程序的内部进行传递，并且广播接收器也只能接收来自本应用程序发出的广播

本地广播的用法并不复杂，主要就是使用了一个 LocalBroadcastManager 来对广播进行管理，并提供了发送广播和注册广播接收器的方法。

```java
public class MainActivity extends Activity {
	private IntentFilter intentFilter;
	private LocalReceiver localReceiver;
	private LocalBroadcastManager localBroadcastManager;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		localBroadcastManager = LocalBroadcastManager.getInstance(this);
		// 获取实例
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent("com.example.broadcasttest.
				LOCAL_BROADCAST");
				localBroadcastManager.sendBroadcast(intent); // 发送本地广播
			}
		});
		intentFilter = new IntentFilter();
		intentFilter.addAction("com.example.broadcasttest.LOCAL_BROADCAST");
		localReceiver = new LocalReceiver();
		localBroadcastManager.registerReceiver(localReceiver, intentFilter);
		// 注册本地广播监听器
	}
	@Override
	protected void onDestroy() {
		super.onDestroy();
		localBroadcastManager.unregisterReceiver(localReceiver);
	}
	class LocalReceiver extends BroadcastReceiver {
		@Override
		public void onReceive(Context context, Intent intent) {
		Toast.makeText(context, "received local broadcast",
		Toast.LENGTH_SHORT).show();
		}
	}
}

```


另外还有一点需要说明，本地广播是无法通过静态注册的方式来接收的。其实这也完全可以理解，因为静态注册主要就是为了让程序在未启动的情况下也能收到广播，而发送本地广播时，我们的程序肯定是已经启动了，因此也完全不需要使用静态注册的功能。

最后我们再来盘点一下使用本地广播的几点优势吧。

1. 可以明确地知道正在发送的广播不会离开我们的程序，因此不需要担心机密数据泄漏的问题。
2. 其他的程序无法将广播发送到我们程序的内部，因此不需要担心会有安全漏洞的隐患。
3. 发送本地广播比起发送系统全局广播将会更加高效。


