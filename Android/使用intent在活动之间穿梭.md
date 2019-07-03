# 使用intent在活动之间穿梭

* **显示intent**

  1. 使用Intent(Context packageContext,Class<?> cls）创建一个intent对象
  2. 调用startActivity(intent)启动活动

  **示例代码**

  ```java
  button.setOnclickListener(new View.OnclickListener(){
      public void onClick(){
      	Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
      	startActivity(intent);
      }
  });
  ```

* **隐式intent**

  1. 在AndroidMainfest.xml中的activity中添加\<intent-filter>标签设置action和category

  2. 使用intent(String action)创建一个intent对象
  3. 使用intent.addCategory(String category)添加category
  4. 调用startActivity(intent)启动活动

  **<font color="red">注意: </font>只有活动的action和category内容同时能够匹配intent中指定的内容时活动才能响应**

  **示例代码**

  ```java
  //设置AndroidMainfest.xml
  <activity name=".SecondActivity">
  	<intent-filter>
  	//action 和 category的android:name内容可以自定义
  		<action android:name="com.example.ACTION_START" />
          <category android:name="android.intent.category.DEFAULT"/>
  	</intent-filter>
  </activity>
  //设置按钮事件
  button.setOnclickListener(new View.OnclickListener(){
  	public void onClick(){
      	Intent intent = new Intent("com.example.ACTION_START");
      	//android.intent.category.DEFAULT是一种默认的category
      	startActivity(intent);
      	}
  });
  ```

* **更多intent的用法**

  * 启动一个浏览器

    ```java
    button.setOnclickListener(new View.OnclickListener(){
        public void onClick(){
        	Intent intent = new Intent(Intent.ACTION_VIEW);
        	//Intent.ACTION_VIEW的值为android.intent.action.VIEW
        	intent.setData(Uri.parse("http://www.baidu.com"));
        	//Uri.parse("协议:值")
        	startActivity(intent);
        }
    });
    ```

  * 拨打电话

    ```java
    button.setOnclickListener(new View.OnclickListener(){
        public void onClick(){
        	Intent intent = new Intent(Intent.ACTION_DIAL);
        	//Intent.ACTION_VIEW的值为android.intent.action.VIEW
        	intent.setData(Uri.parse("tel:10086"));
        	startActivity(intent);
        }
    });
    ```

    