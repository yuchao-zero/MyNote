# Mac 系统下的 /Library，/System/Library，~/Library 这三个分别有什么不同的用法？

1. ／System／library是系统级，/ Library下面面向全部用户，~du/Library 限于当前用户
2. 一般来说，很少碰/ Library，都是用到/System/library和~/library吧，有些软件安装的时候，选择所有用户可用，就装在／library下面，不然应该是在~/library下面
3. java在/Library下
4. tomcat在~/library下