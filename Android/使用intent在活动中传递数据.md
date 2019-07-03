# 使用intent在活动中传递数据

* **向下一个活动中传递数据**

  ```java
  //FirstActivity中
  button.setOnclickListener(new View.OnclickListener(){
      public void onClick(){
          String data = "test";
          Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
          intent.putExtra("data",data);
          startActivity(intent);
      }
  });
  //SecondActivity中
  protected void onCreate(Bundle savedInstanceState){
      super.onCreate(savedInstanceState);
      setContentView(R.layout.second_layout);
      Intent intent = getIntent();//获取启动该活动的intent
      String data = intent.getStringExtra("data");//获取int类型的用getIntExtra()以此类推
      Log.d("SecondActivity",data);
  }
  ```

* 返回数据给上一个活动

  ```java
  //FirstActivity中
  button.setOnclickListener(new View.OnclickListener(){
      public void onClick(){
          Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
          //请求码只要是个唯一值就行这里传入了1
          startActivityForResult(intent,1);
      }
  });
  //重写FirstActivity的onActivityResult()
  protected void onActivityResult(int requestCode,int resultCode,Intent data){
      switch(requestCode){
          case 1:
              if(resultCode == RESULT_OK){
                  String res = data.getStringExtra("data");
                  Log.d("FirstActivity",res);
              }
              break;
      }
  }
  //SecondActivity中
  protected viod onCreate(Bundle savedInstanceState){
      super.onCreate(savedInstanceState);
      setContentView(R.layout.second_layout);
      ...
       button.setOnClickListener(new View.OnclickListener(){
           public void onClick(View v){
               Intent intent = new Intent();
               intent.putExtra("data","test");
               setResult(RESULT_OK,intent);
               finish();
           }
       }); 
  }
  //当用户点了返回键时
  public void onBackPressed(){
      Intent intent = new Intent();
      intent.putExtra("data","test");
      setResult(RESULT_OK,intent);
      finish();
  }
  
  ```

  