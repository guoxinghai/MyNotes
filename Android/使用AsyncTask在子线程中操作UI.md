# 使用`AsyncTask`在子线程中操作`UI`

> `Android`不允许在子线程中对`UI`进行操作,我可以借助`AsyncTask`实现对`UI`得操作,`AsyncTask`得实现原理是基于异步消息处理机制的,`Android`帮我们做好了封装。

`AsyncTask`是一个抽象类,所以我们必须创建一个子类去继承它，在继承时我们可以为`AsyncTask`指定3个泛型参数,3个参数的用途如下:

* `Params`

  > 在执行`AsyncTask`时需要传入的参数，可用于后台任务的使用。`void`表示不需要传参数给后台任务。

* `Progress`

  > 在后台任务执行时，如果需要在界面显示当前进度，则指定泛型作为进度单位。void`表示不需要显示当前进度。

* `Result`

  > 当任务执行完毕后，如果需要对结果进行返回，则使用这里指定泛型作为返回值类型。void`表示不需要对结果进行返回。

一般需要重写的方法由4个：

* `onPreExecute()`

  > 这个方法会在后台任务开始执行之前调用，用于进行一些界面上的初始化操作，比如显示一个进度条对话框等。

* `doInBackground(Params...)`

  > 这个方法中的代码会在子线程中运行，我们应该在这里去处理所有耗时的任务。任务一旦完成就可以通过`return`语句来将任务的执行结果返回，如果`AsyncTask`的第三个泛型参数为`void`则不需要返回结果。注意在这里是不可以进行`UI`操作的。如果需要可以调用`publishProgress(progress...)`方法来完成`UI`操作。

* `onProgressUpdate(progress...)`

  > 当在后台任务中调用了`publishProgress(Progress...)`方法后，`onProgressUpdate(progress...)`方法就会很快被调用,利用参数中的数值就可以对界面元素进行响应更新。

* `onPostExecute(Result)`

  > 当后台任务执行完毕并通过`return`语句返回时，这个方法很快会被调用。返回的数据会作为参数传递到此方法中，可以利用返回的数据进行一些`UI`操作，比如提醒任务执行结果，以及关闭进度条等。

一个完整的自定义`AsyncTask`就可以写成如下方式:

```java
class DownloadTask extends AsyncTask<void,Integer,Boolean>{
    @Override
    protected void onPreExecute(){
        progressDialog.show();//显示进度对话框
    }
    @Override
    protected Boolean doInBackground(Void...params){
        try{
            while(true){
                int downloadPercent = doDownload();//这是一个虚构方法
                publishProgress(downloadPercent);
                if(downloadPercent >= 100){
                    break;
                }
            }
        }catch(Exception e){
                return false;
        }
        return true;
    }
   @Override
   protected void onProgressUpdate(Integer...values){
       //在这里更新下载进度
       progressDialog.setMessage("Download "+ values[0] + "%");
   }
   @Override
   protected void onPostExecute(Boolean result){
       progressDialog.dismiss();//关闭对话框
       //在这里提示下载结果
       if(result){
          ... 
       }else{
           ...
       }
   }
}
```

