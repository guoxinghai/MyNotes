# WebView用法

> 如果要在应用程序中展示一些网页。可以借助`WebView`控件，借助它我们就可以在自己得应用程序中嵌入一个浏览器。

1. 在`layout`得布局文件中添加`WebView`控件

   ```xml
   <WebView
            android:id=""
            android:layout_width=""
            android::layout_height="" />
   ```

2. 在`activity`中配置`WebView`

   ```javafind
   WebView webView = (WebView)findViewById(R.id.webView);
   //让WebView支持javascript脚本
   webView.getSettings().setJavaScriptEnabled(true);
   //下面这段代码得作用是当需要跳转到另一网页时我们希望目标网页仍然在当前WebView中显示
   webView.setWebViewClient(new WebViewClient());
   //传入要显示得网页地址
   webView.loadUrl("http://www.baidu.com");
   ```

3. 由于使用到了网络功能，而访问网络是需要权限的，因此还需要修改`AndroidManifest.xml`,加入权限

   ```xml
   <manifest>
       <uses-permission android:name="android:permission.INTERNET" />
   </manifest>
   ```

   