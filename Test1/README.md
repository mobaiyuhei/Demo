# README

## 测试介绍

主要是测试在手机上面查看全景照片

使用的是Google的api

## 实现
### build.gradle引入项目
```
 compile 'com.google.vr:sdk-panowidget:1.10.0'
```
== 注意:这个要求sdk必须是 > 19 ==

### 布局文件中添加
```
<com.google.vr.sdk.widgets.pano.VrPanoramaView
        android:id="@+id/vr_pan_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="20dp" />
 ```
 
 ### java代码
 ```
 /**
     * 加载360度全景图片
     */
    private void load360Image() {
        vr_pan_view = (VrPanoramaView) findViewById(R.id.vr_pan_view);
        /**获取assets文件夹下的图片**/
        InputStream open = null;
        try {
            open = getAssets().open("test.jpg");
        } catch (IOException e) {
            e.printStackTrace();
        }
        Bitmap bitmap = BitmapFactory.decodeStream(open);
        /**设置加载VR图片的相关设置**/
        VrPanoramaView.Options options = new VrPanoramaView.Options();
        options.inputType = VrPanoramaView.Options.TYPE_STEREO_OVER_UNDER;
        /**设置加载VR图片监听**/
        vr_pan_view.setEventListener(new VrPanoramaEventListener() {
            /**
             * 显示模式改变回调
             * 1.默认
             * 2.全屏模式
             * 3.VR观看模式，即横屏分屏模式
             * @param newDisplayMode 模式
             */
            @Override
            public void onDisplayModeChanged(int newDisplayMode) {
                super.onDisplayModeChanged(newDisplayMode);
                Log.d(TAG, "onDisplayModeChanged()->newDisplayMode=" + newDisplayMode);
            }

            /**
             * 加载VR图片失败回调
             * @param errorMessage
             */
            @Override
            public void onLoadError(String errorMessage) {
                super.onLoadError(errorMessage);
                Log.d(TAG, "onLoadError()->errorMessage=" + errorMessage);
            }

            /**
             * 加载VR图片成功回调
             */
            @Override
            public void onLoadSuccess() {
                super.onLoadSuccess();
                Log.d(TAG, "onLoadSuccess->图片加载成功");
            }

            /**
             * 点击VR图片回调
             */
            @Override
            public void onClick() {
                super.onClick();
                Log.d(TAG, "onClick()");
            }
        });
        /**加载VR图片**/
        vr_pan_view.loadImageFromBitmap(bitmap, options);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        /**关闭加载VR图片，释放内存**/
        if (vr_pan_view != null&& vr_pan_view.isActivated())
            vr_pan_view.shutdown();
    }
 ```
 