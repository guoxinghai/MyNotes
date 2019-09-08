# 使用`OkHttp`

>   `OkHttp`不仅在接口上封装上做的简单易用，就连在底层实现上也是自成一派，比起原生得`HttpURLConnection`,可以说是有过之而无不及。

**<font color="red">注意:</font>发送`Http`请求而非`Https`OkHttp3会报**`CLEARTEXT communication to xxx not permitted by network security policy`**解决方案在文章末尾**



1. **配置**

   在`app/build.gradle`中添加依赖库

   ```xml
   dependencies{
   	implementation 'com.squareup.okhttp3:okhttp:3.4.1'
   }
   ```

2. **GET请求**

   1. 创建`OkhttpClient`

      `OkHttpClient client = new OkHttpClient();`

   2. 创建`request`

      ```java
      Request request = new Request.Builder()
          				.url("http://www.baidu.com")
          				.build();
      ```

   3. 发送请求

      `Response response = client.newCall(request).execute();`

   4. 获取请求数据

      `String data = respnse.body().string();`

3. **POST请求**

   ```java
   //创建OkHttpClient
   OkHttpClient client = new OkHttpClient();
   //创建请求体对象
   RequestBody requestBody = new FormBody.Builder()
       .add("username","admin")
       .add("password","123456")
       .build();
   //创建求情对象
   Request request = new Request.Builder()
       .url("http://ww.baidu.com")
       .post(requestBody)
       .build();
   //发送请求
   Response response = client.newCall(request).execute();
   //获取返回数据
   String data = respnse.body().string();
   ```




**`OkHttp`访问`Http`协议网站报错解决方案**

1. 创建`res/xml/network_security_config.xml`文件，内容如下：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <network-security-config>
       <base-config cleartextTrafficPermitted="true" />
   </network-security-config>
   ```

2. 在`AndroidManifest.xml`下的application标签增加`networkSecurityConfig`属性：

   ```xml
   <application 
       android:networkSecurityConfig="@xml/network_security_config"
   >
   <!-- -->
   </application>
   ```

   