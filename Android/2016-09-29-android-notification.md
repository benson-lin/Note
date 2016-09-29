# 使用通知

通知（ Notification）是 Android 系统中比较有特色的一个功能，当某个应用程序希望向用户发出一些提示信息，而该应用程序又不在前台运行时，就可以借助通知来实现。发出一条通知后，手机最上方的状态栏中会显示一个通知的图标，下拉状态栏后可以看到通知的详细内容。 



## 通知的基本用法

通知的用法还是比较灵活的， 既可以在活动里创建，也可以在广播接收器里创建，当然还可以在下一章中我们即将学习的服务里创建。相比于广播接收器和服务，在活动里创建通知的场景还是比较少的，因为一般只有当程序进入到后台的时候我们才需要使用通知。


不过，无论是在哪里创建通知，整体的步骤都是相同的，下面我们就来学习一下创建通知的详细步骤。首先需要一个 NotificationManager 来对通知进行管理，可以调用 Context 的getSystemService()方法获取到。 getSystemService()方法接收一个字符串参数用于确定获取系统的哪个服务，这里我们传入 Context.NOTIFICATION_SERVICE 即可。因此，获取NotificationManager 的实例就可以写成：

```java
NotificationManager manager = (NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);
```

接下来需要创建一个 Notification 对象，这个对象用于存储通知所需的各种信息，我们可以使用它的有参构造函数来进行创建。 使用Buider创建。

```java
Notification notification = new Notification.Builder(MainActivity.this)
                                        .setTicker("The ticker")
                                        .setContentTitle("Title")
                                        .setContentText("Context")
                                        .setSmallIcon(R.drawable.icon)
                                        .setLargeIcon(BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.icon))
                                        .setAutoCancel(true)//设置可以清除
                                        .setContentIntent(pi)
                                        .build();
```


以上工作都完成之后，只需要调用 NotificationManager 的 notify()方法就可以让通知显示出来了。 notify()方法接收两个参数，第一个参数是 id，要保证为每个通知所指定的 id 都是不同的。第二个参数则是 Notification 对象，这里直接将我们刚刚创建好的 Notification 对象传入即可。因此，显示一个通知就可以写成：

```java
manager.notify(1, notification);
```

## 实例

新建一个 NotificationTest 项目，并修改 activity_main.xml 中的代码

```xml
    <Button
        android:id="@+id/send_notice"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
```

布局文件非常简单，里面只有一个 Send notice 按钮，用于发出一条通知。接下来修改MainActivity 中的代码

```java
 sendNoticeButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                switch (v.getId()){
                    case R.id.send_notice:
                        NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);

                        Notification notification = new Notification.Builder(MainActivity.this)
                                .setTicker("The ticker")
                                .setContentTitle("Title")
                                .setContentText("Context")
                                .setSmallIcon(R.drawable.icon)
                                .setLargeIcon(BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.icon))
                                .setAutoCancel(true)//设置可以清除
                                .build();
                        manager.notify(1, notification);
                        break;
                    default:
                        break;

                }
            }
        });
```

如果你使用过 Android 手机，此时应该会下意识地认为这条通知是可以点击的。但是当你去点击它的时候，你会发现没有任何效果。不对啊，好像每条通知点击之后都应该会有反应的呀？其实要想实现通知的点击效果，我们还需要在代码中进行相应的设置，这就涉及到了一个新的概念， PendingIntent。

## PendingIntent

PendingIntent 从名字上看起来就和 Intent 有些类似，它们之间也确实存在着不少共同点。比如它们都可以去指明某一个“意图”，都可以用于启动活动、启动服务以及发送广播等。不同的是， Intent 更加倾向于去立即执行某个动作，而 PendingIntent 更加倾向于在某个合适的时机去执行某个动作。所以，也可以把 PendingIntent 简单地理解为延迟执行的 Intent。

PendingIntent 的用法同样很简单，它主要提供了几个静态方法用于获取 PendingIntent 的实例，可以根据需求来选择是使用 getActivity()方法、 getBroadcast()方法、还是 getService()方法。这几个方法所接收的参数都是相同的，第一个参数依旧是 Context，不用多做解释。第二个参数一般用不到，通常都是传入 0 即可。第三个参数是一个 Intent 对象，我们可以通过这个对象构建出 PendingIntent 的“意图”。第四个参数用于确定 PendingIntent 的行为，有FLAG_ONE_SHOT、 FLAG_NO_CREATE、 FLAG_CANCEL_CURRENT 和 FLAG_UPDATE_CURRENT 这四种值可选，每种值的含义你可以查看文档，我就不一一进行解释了。


```java
NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
                                Intent intent = new Intent(MainActivity.this, NotificationActivity.class);
                                PendingIntent pi = PendingIntent.getActivity(MainActivity.this, 0, intent, PendingIntent.FLAG_CANCEL_CURRENT);

                                Notification notification = new Notification.Builder(MainActivity.this)
                                        .setTicker("The ticker")
                                        .setContentTitle("Title")
                                        .setContentText("Context")
                                        .setSmallIcon(R.drawable.icon)
                                        .setLargeIcon(BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.icon))
                                        .setAutoCancel(true)//设置可以清除
                                        .setContentIntent(pi)
                                        .build();
 manager.notify(1, notification);
```

现在我们来优化一下 NotificationTest 项目，给刚才的通知加上点击功能，让用户点击它的时候可以启动另一个活动，也就是NotificationActivity.java。首先需要准备好另一个活动，这里新建布局文件 notification_layout.xml

```xml
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp"
        android:text="This is notification layout"/>
```

布局文件的内容非常简单，只有一个居中显示的 TextView，用于展示一段文本信息。然后新建 NotificationActivity 继承自 Activity，在这里加载刚才定义的布局文件

```java
public class NotificationActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.notification_layout);
    }
}
```

然后修改 AndroidManifest.xml 中的代码，在里面加入 NotificationActivity 的注册声明

```xml
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name" >
            ...
        </activity>
        <activity android:name=".NotificationActivity" />
    </application>
```

然后再将 MainActivity.java修改成上述的代码就可以了


## 发出声音/LED灯控制/手机震动

观察 Notification这个类，你会发现里面还有很多我们没有使用过的属性。先来看看 sound这个属性吧，它可以在通知发出的时候播放一段音频，这样就能够更好地告知用户有通知到来。 sound 这个属性是一个 Uri 对象，所以在指定音频文件的时候还需要先获取到音频文件对应的 URI。比如说，我们手机的/system/media/audio/ringtones 目录下有一个 Basic_tone.ogg音频文件，那么在代码中这样就可以这样指定：

```java
Uri soundUri = Uri.fromFile(new File("/system/media/audio/ringtones/
Basic_tone.ogg"));
notification.sound = soundUri;
```

除了允许播放音频外，我们还可以在通知到来的时候让手机进行振动，使用的是 vibrate这个属性。它是一个长整型的数组，用于设置手机静止和振动的时长，以毫秒为单位。下标为 0 的值表示手机静止的时长，下标为 1 的值表示手机振动的时长，下标为 2 的值又表示手机静止的时长，以此类推。所以，如果想要让手机在通知到来的时候立刻振动 1 秒，然后静止 1 秒，再振动 1 秒，代码就可以写成：

```java
long[] vibrates = {0, 1000, 1000, 1000};
notification.vibrate = vibrates;
```

不过，想要控制手机振动还需要声明权限的。因此，我们还得编辑 AndroidManifest.xml
文件，加入如下声明：

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="com.zebin.notificationtest"
	android:versionCode="1"
	android:versionName="1.0" >
……
<uses-permission android:name="android.permission.VIBRATE" />
……
</manifest>
```

学会了控制通知的声音和振动，下面我们来看一下如何在通知到来时控制手机 LED 灯的显示。

现在的手机基本上都会前置一个 LED 灯，当有未接电话或未读短信，而此时手机又处于锁屏状态时，LED 灯就会不停地闪烁，提醒用户去查看。我们可以使用 ledARGB、ledOnMS、ledOffMS 以及 flags 这几个属性来实现这种效果。 ledARGB 用于控制 LED 灯的颜色，一般有红绿蓝三种颜色可选。 ledOnMS 用于指定 LED 灯亮起的时长，以毫秒为单位。 ledOffMS用于指定 LED 灯暗去的时长，也是以毫秒为单位。 flags 可用于指定通知的一些行为，其中就包括显示 LED 灯这一选项。所以，当通知到来时，如果想要实现 LED 灯以绿色的灯光一闪一闪的效果，就可以写成：

```java
notification.ledARGB = Color.GREEN;
notification.ledOnMS = 1000;
notification.ledOffMS = 1000;
notification.flags = Notification.FLAG_SHOW_LIGHTS;
```

当然，如果你不想进行那么多繁杂的设置，也可以直接使用通知的默认效果，它会根据当前手机的环境来决定播放什么铃声，以及如何振动，写法如下：

```java
notification.defaults = Notification.DEFAULT_ALL;
```

注意，以上所涉及的这些高级技巧都要在手机上运行才能看得到效果，模拟器是无法表现出振动、以及 LED 灯闪烁等功能的。




## 代码总结

```java

public class MainActivity extends Activity {

    private Button sendNoticeButton;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        sendNoticeButton = (Button)findViewById(R.id.send_notice);
        sendNoticeButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                switch (v.getId()){
                    case R.id.send_notice:
                        Runnable runnable = new Runnable() {
                            @Override
                            public void run() {
                                try {
                                    Thread.sleep(4000);
                                } catch (InterruptedException e) {
                                    e.printStackTrace();
                                }
                                NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
                                Intent intent = new Intent(MainActivity.this, NotificationActivity.class);
                                PendingIntent pi = PendingIntent.getActivity(MainActivity.this, 0, intent, PendingIntent.FLAG_CANCEL_CURRENT);

                                Notification notification = new Notification.Builder(MainActivity.this)
                                        .setTicker("The ticker")
                                        .setContentTitle("Title")
                                        .setContentText("Context")
                                        .setSmallIcon(R.drawable.icon)
                                        .setLargeIcon(BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.icon))
                                        .setAutoCancel(true)//设置可以清除
                                        .setContentIntent(pi)
                                        .build();
                                notification.defaults = Notification.DEFAULT_SOUND;//设置为默认的声音,不写没声音
                                long[] vibrates = {0, 1000, 1000, 1000};
                                notification.vibrate = vibrates;
                                //LED灯
                                notification.ledARGB = Color.GREEN;
                                notification.ledOnMS = 1000;
                                notification.ledOffMS = 1000;
                                notification.flags = Notification.FLAG_SHOW_LIGHTS;
                                manager.notify(1, notification);
                            }
                        };
                        new Thread(runnable).start();
                        break;
                    default:
                        break;

                }
            }
        });

    }
```