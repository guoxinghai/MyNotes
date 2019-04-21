# response.sendRedirect()和request.getRequestDispatcher().forward()的区别

1. 意义不同

   * response.sendRedirect(url)是重定向到指定的url

   * request.getRequestDispatcher().forward(request, response)是将请求转发到指定的url

2. 跳转单位不同

   * response.sendRedirect(url)是客户端的跳转

   * response.getRequestDispatcher(url).forward(request,response)是服务器的跳转

3. 跳转之后

   * response.sendRedirect()在跳转之后，上个页面中的请求全部结束,原request对象将会消亡

     数据将会消失。紧接着，当新页面会新建request对象。

     【详细过程：redirect 会首先发一个response给浏览器，然后浏览器收到这个response后再发一个requeset给服务器，服务器接收后发新的response给浏览器。这时页面从浏览器获取来的是一个新的request。这时，在原来跳转之前的页面用request.setAttribute存的东西都没了，如果在当前的新页面中用request.getAttribute取，得到的将会是null。】

   * request.getRequestDispatcher(url).forward(request,response)是采用请求转发方式，在跳转页面的时候是带着原来页面的request和response跳转的，request对象始终存在，不会重新创建。

     【详细过程：forward 发生在服务器内部, 是在浏览器完全不知情的情况下发给了浏览器另外一个页面的response. 这时页面收到的request不是从浏览器直接发来的，可能是在转页时己经用request.setAttribute在request里放了数据，在转到的页面就可以直接用request.getAttribute获得数据了。】

4. 地址栏
   * response.senRedirect(url)的地址栏发生了改变
   * response.getDispatcher(url).forward(request,response)的地址栏未发生改变

5. 使用范围
   * response.senRedirect(url)可以重定向到任何URL
   * response.getRequestDispatcher(url).forward(request,response)只能定位到web应用程序中的资源