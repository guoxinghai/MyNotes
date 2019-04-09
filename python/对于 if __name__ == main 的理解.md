# 对于 if \_\_name\_\_ == __main__ 的理解

1. 在test包中创建一个demo的.py文件里面包含如下代码

   ```python
   print('first')
   print(__name__)
   if __name__ == '__main__':
       print('second')
   ```

   运行之后结果如下：

   ```python
   first
   __main__
   second
   ```

2. 创建一个import_demo的 .py文件包含如下代码

   ```python
   from test import demo
   print('test')
   ```

   运行结果如下:

   ```python
   first
   test.demo
   test
   ```

   `if`语句中的内容没有运行

3. 说明\_\_name\_\_

   * 运行demo文件时\_\_name\_\_为\_\_main\_\_

   * 运行import_demo时\_\_name\_\_为test.demo

     > 举个例子小明的**名字**对于他自己来说是我但对于别人来说就是**小明**

