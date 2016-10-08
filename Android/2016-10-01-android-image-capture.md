# 调用摄像头和相册

## 调用摄像头拍照

很多应用程序都可能会使用到调用摄像头拍照的功能，比如说程序里需要上传一张图片作为用户的头像，这时打开摄像头拍张照是最简单快捷的。下面就让我们通过一个例子来学习一下，如何才能在应用程序里调用手机的摄像头进行拍照。


新建一个 ChoosePicTest 项目，然后修改 activity_main.xml 中的代码，如下所示：

```xml
    <Button
        android:id="@+id/take_photo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Take Photo" />

    <ImageView
        android:id="@+id/picture"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:background="#f0f"/>
```

可以看到，布局文件中只有两个控件，一个 Button 和一个 ImageView。 Button 是用于打开摄像头进行拍照的，而 ImageView 则是用于将拍到的图片显示出来。


然后开始编写调用摄像头的具体逻辑，修改 MainActivity 中的代码，如下所示

```java
public class MainActivity extends Activity {

    public static final int TAKE_PHOTO = 1;
    public static final int CROP_PHOTO = 2;
    private Button takePhoto;
    private ImageView picture;
    private Uri imageUri;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        takePhoto = (Button) findViewById(R.id.take_photo);
        picture = (ImageView) findViewById(R.id.picture);
        takePhoto.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
// 创建File对象，用于存储拍照后的图片
                File outputImage = new File(Environment.
                        getExternalStorageDirectory(), "tempImage.jpg");
                try {
                    if (outputImage.exists()) {
                        outputImage.delete();
                    }
                    outputImage.createNewFile();
                } catch (Exception e) {
                    e.printStackTrace();
                }
                imageUri = Uri.fromFile(outputImage);
                Intent intent = new Intent("android.media.action.IMAGE_CAPTURE");
                intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri);
                startActivityForResult(intent, TAKE_PHOTO); // 启动相机程序
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case TAKE_PHOTO:
                if (resultCode == RESULT_OK) {
                    Intent intent = new Intent("com.android.camera.action.CROP");
                    intent.setDataAndType(imageUri, "image/*");
                    intent.putExtra("scale", true);
                    intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri);
                    startActivityForResult(intent, CROP_PHOTO); // 启动裁剪程序
                }
                break;
            case CROP_PHOTO:
                if (resultCode == RESULT_OK) {
                    try {
                        Bitmap bitmap = BitmapFactory.decodeStream
                                (getContentResolver()
                                        .openInputStream(imageUri));
                        picture.setImageBitmap(bitmap); // 将裁剪后的照片显示出来
                    } catch (FileNotFoundException e) {
                        e.printStackTrace();
                    }
                }
                break;
            default:
                break;
        }
    }
}

```

上述代码稍微有点复杂，我们来仔细地分析一下。在 MainActivity 中要做的第一件事自然是分别获取到 Button 和 ImageView 的实例，并给 Button 注册上点击事件，然后在 Button的点击事件里开始处理调用摄像头的逻辑，我们重点看下这部分代码。


首先这里创建了一个 File 对象，用于存储摄像头拍下的图片，这里我们把图片命名为 output_image.jpg ， 并 将 它 存 放 在 手 机 SD 卡 的 根 目 录 下 ， 调 用 Environment 的 getExternalStorageDirectory()方法获取到的就是手机 SD 卡的根目录。然后再调用 Uri 的 fromFile()方法将 File 对象转换成 Uri 对象，这个 Uri 对象标识着 output_image.jpg 这张图片的唯一地址。接着构建出一个 Intent对象，并将这个 Intent的 action指定为 android.media.action.IMAGE_CAPTURE，再调用 Intent 的 putExtra()方法指定图片的输出地址，这里填入刚刚得到的 Uri 对象，最后调用 startActivityForResult()来启动活动。由于我们使用的是一个隐式Intent，系统会找出能够响应这个 Intent 的活动去启动，这样照相机程序就会被打开，拍下的照片将会输出到 output_image.jpg 中。

注意刚才我们是使用 startActivityForResult()来启动活动的，因此拍完照后会有结果返回到 onActivityResult()方法中。如果发现拍照成功，则会再次构建出一个 Intent 对象，并把它的 action 指定为com.android.camera.action.CROP。这个 Intent 是用于对拍出的照片进行裁剪的，因为摄像头拍出的照片都比较大，而我们可能只希望截取其中的一小部分。然后给这个Intent 设置上一些必要的属性，并再次调用startActivityForResult()来启动裁剪程序。裁剪后的照片同样会输出到 output_image.jpg 中。裁剪操作完成之后，程序又会回调到 onActivityResult()方法中，这个时候我们就可以调用 BitmapFactory 的 decodeStream()方法将 output_image.jpg 这张照片解析成 Bitmap 对象，然后把它设置到 ImageView 中显示出来。


由于这个项目涉及到了向 SD 卡中写数据的操作，因此我们还需要在 AndroidManifest.xml中声明权限：


```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```


这样代码就都编写完了，现在将程序运行到手机上，然后点击 Take Photo 按钮就可以进行拍照了


## 从相册中选择照片


## 不进行裁剪

```xml
<Button
android:id="@+id/choose_from_album"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="Choose From Album" />
```

```java
 chooseFromAlbum = (Button) findViewById(R.id.choose_from_album);
        chooseFromAlbum.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_PICK,
                       android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
                startActivityForResult(intent, CROP_PHOTO);

            }
        });
```

```java
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case CROP_PHOTO:
                if (resultCode == RESULT_OK) {
                    try {
                        Bitmap bitmap = BitmapFactory.decodeStream
                                (getContentResolver()
                                        .openInputStream(data.getData()));
                        picture.setImageBitmap(bitmap); // 将裁剪后的照片显示出来
                    } catch (FileNotFoundException e) {
                        e.printStackTrace();
                    }
                }
                break;
            default:
                break;
        }
    }
```

intent.getData() 获取选中的手机图片URI地址


## 需要进行裁剪

需要保存到临时文件