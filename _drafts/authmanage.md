

### Function

**Function**：系统提供的功能。一个系统的function，可能是系统中的一个方法，也可能是有系统中一系列的方法。在权限控制的粒度来说，它已经很接近权限控制的最细粒度了，控制用户的权限也就是控制用户可以访问系统的哪些方法，在权限系统概念中也就是访问哪些功能。所以funciton的概念`就是代表系统中的一个方法或一些列方法`。

**最细粒度**：此外需要强调和申明的上文是说function已经很`接近`权限控制可以达到的最细粒度了。但有时我们可能会遇到一些情况，我们不仅仅要控制用户访问哪些功能```(即是可以执行哪些方法)```,我们还要控制到方法内部。例如淘宝商城每天会出一份各地的交易报表，我们只希望各地运营管理员只可以看到自己负责地域的报表。这时展示报表就是一个功能，各地管理员都应该有这么一个功能，只是要控制到方法内部展示的数据范围。考虑到这种情景下，我认为能够控制到方法的内部，才可以算的上是最细粒度的控制。在1.0 版本中支持的最小粒度就是功能，2.0版本会考虑增加方法内部的粒度控制

### Resource
**Resource**:系统提供的资源。一个系统的资源就是一个url或者是url的一部分。在权限控制系统中设置该功能是直接通过url来限制用户的访问，这种控制访问的粒度比较大，控制的方式也相对于Function来说比较粗糙，在2.0版本里，已经不建议大范围使用该功能角色去限制。它只是对Function起一定得辅助功能。

**辅助Function**：Resource是怎么辅助Function的设计呢？上文提到Function的设计是代表系统中的一个功能，它可以控制的粒度是方法，你只需要在方法上加上一个注解表明这个方法是这个功能或是这个功能的一部分即可。这些操作是在开发者在编写程序时添加好的。当程序上线后，这时有个功能，比如给客户发送消息，在开发者设计之初并没有要给他加入权限控制。但程序已经上线，这时第一种情况，就是在权限系统登记该功能，然后开发者在相应的方法加上该功能的注解配置，然后重新上线。但是老板说今天是周五，不允许重新上线。这时你就可以利用第二种方案，利用Resource直接把这个给用户发送消息的接口的api允许只有权限的人去访问。


### User
**User**:系统的使用用户。


### role

### system

