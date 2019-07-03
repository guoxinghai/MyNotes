# RecyclerView使用介绍

>   RecyclerView是一个比ListView更强大的组件，不仅实现了ListView同样的效果，还优化了ListView中存在的各种不足之处。并且相比于ListView，Android官方更推荐使用RecyclerView。

* **配置build.grade**(Android Studio)

  > 要使用RecyclerView需要再builde.gradle中添加响应的依赖库

  打开**`app/build.gradle`**文件,在**`dependencies`**中添加如下内容(基于Android **SDK 28**)

  **`implementation 'com.android.support:recyclerview-v7:28.0.0'`**

  然后点击`Sync Now`来同步

* 在布局中引入RecyclerView

  引入这个组件要使用完全限定性名**`android.support.v7.widget.RecyclerView`**

  例如

  ```java
  <android.support.v7.widget.RecyclerView
  	android:id="@+id/recyclerView"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"/>
         
  ```

* 适配器介绍

  > RecyclerView要配合适配器来加载内容

  **以一份适配器代码来介绍**

  ```java
  public class Adapter extends RecyclerView.Adapter<Adapter.ViewHolder> {
      //用来存取需要加载的数据的集合对象 这份代码的集合用于存储消息对象
      List<Message> dataList = new ArrayList<Message>();
  	//构造方法只需将数据集合传递进来即可
      public Adapter(List<Message> messages){
          this.messageList = messages;
      }
     
      //onCreateViewHolder()函数是用来创建ViewHolder对象的（关于ViewHolder会在下面介绍）
      @NonNull
      @Override
      public ViewHolder onCreateViewHolder(@NonNull ViewGroup viewGroup, int i) {
          
          View view = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.msg_layout,viewGroup,false);
          return new ViewHolder(view);
      }
  
      //onBindViewHolder()函数是用来将数据绑定到ViewHolder中的属性的，也可以在这为部件设置监听等事	//件
      @Override
      public void onBindViewHolder(@NonNull ViewHolder viewHolder, int i) {
          final Message message = messageList.get(i);
          if(message.getFlag()==Message.LEFT_MESSAGE){
              viewHolder.rightText.setVisibility(View.GONE);
              viewHolder.leftText.setText(message.getText());
          }else{
              viewHolder.leftText.setVisibility(View.GONE);
              viewHolder.rightText.setText(message.getText());
          }
          viewHolder.itemView.setOnClickListener(new View.OnClickListener(){
  
              @Override
              public void onClick(View v) {
                  Toast.makeText(v.getContext(),message.getText(),Toast.LENGTH_SHORT).show();
              }
          });
      }
  
      //getItemCount()获取集合对象的大小
      @Override
      public int getItemCount() {
          return messageList.size();
      }
      
  	/*
  	* 下面是ViewHolder类继承自RecyclerView.ViewHolder类,
  	* ViewHolder用于存储与某个视图对象有关的数据
  	* 使用View.setTag()来设置ViewHolder
  	* 使用View.getTag()来获取ViewHolder
  	* 这里使用ViewHolder是为了减少View.findViewById()方法的使用
  	*/
      class ViewHolder extends RecyclerView.ViewHolder {
          TextView leftText;
          TextView rightText;
          View itemView;
          //构造函数获取一个itemView即cyclerView的布局子对象
          public ViewHolder(@NonNull View itemView) {
              super(itemView);
              this.itemView = itemView;
              leftText = (TextView)itemView.findViewById(R.id.leftText);
              rightText = (TextView)itemView.findViewById(R.id.rightText);
          }
      }
  }
  ```

* **RecyclerView设置**

  > RecyclerView需要一个适配器，一个布局管理器

  例如在一个活动中设置RecyclerView实现垂直滚动效果

  ```java
  RecyclerView recyclerView = (RecyclerView)findViewById(R.id.recycler_view);
  LinearLayoutManager layoutManager = new LinearLayoutManager(this);
  Adapter adapter = new Adapter(list);
  recyclerView.setAdapter(adapter);
  recyclerView.setLayoutManager(manager);
  ```

  例如在一个活动中设置RecyclerView实现水品滚动效果

  ```java
  RecyclerView recyclerView = (RecyclerView)findViewById(R.id.recycler_view);
  LinearLayoutManager layoutManager = new LinearLayoutManager(this);
  //设置水平滚动
  layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
  
  Adapter adapter = new Adapter(list);
  recyclerView.setAdapter(adapter);
  recyclerView.setLayoutManager(manager);
  ```

  

  