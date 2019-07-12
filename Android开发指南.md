## Android开发指南

1. 沙箱基本都是插件化的变种，参考DroidPlugin，就是一个沙箱。图中的东西就是唬外行用的，Android在没有root的情况下，不可能创建和系统平行的沙盒。
2. 滴滴的屏幕适配方案是哪个
   * 屏幕适配没有通用的方案，滴滴也没啥特殊的方案，就是开发人员看自己喜好，我喜欢用dp。
   * github上有个7000star的方案，AndroidAutoSize
3. 插件化和热修复行业里哪个用的比较多
   * Tinker和VirtualAPK
4. 发起多个异步请求后，有什么好的方式保证其回调的数据按请求的顺序加载？
   * 自己用线程池做排队，加上锁，用锁来控制同步，这个目测没有现成的
5. 最近有个耗电排行的需求，具体这个有什么资料或者源码可参考的吗
   * <https://github.com/google/battery-historian>
   * <https://blog.csdn.net/zhangcanyan/article/details/82999003>
6. h5调用原生的拍照或者相册将图片展示到h5控件上
   * 这个需要Webview来支持，不能通过jsbridge来做。参考<https://www.jianshu.com/p/877eb9ace1d3>
7. 抓包是用什么工具的？
   * fiddler或者charles
8. 大家有没有好的视频压缩方案，自己使用的SiliCompressor，10分钟的视频200M压缩到50M需要6min.有没有更好的方案？？
   - fishwjy/videocompressor
9. 怎样实现让子view滑动一段距离后，父view也开始滑动
   - NestedScroll机制
10. 最近在看 LruCache，想自己用 RecyclerView 实现一下瀑布流照片墙
    * recyclerview四层缓存，我记得只有第一层scrab和第二层cache数据是干净的，不需要重新绑定。RecyclerView缓存的是View，Lrucache缓存的是图片，而且，lrucache集成在图片框架里了，不需要单独调用
11. 带有分享功能的app，点击分享的时候弹出带有分享接受功能的App列表，是如何知道某个App可以接受分享的；选择一个App进行分享的时候，是怎样启动目标App的？
    * 第三方APP留了接口和方法的，比如微信的你需要去看微信的开发者文档，上面会说明怎么调起分享
      *  分享功能就是集成的第三方分享功能，要分享到哪个APP就集成那个APP的分享功能啊，到对应的开放平台去注册应用，集成sdk，调用对应的代码就可以啦
12. 如果插件工程中需要某个动态申请权限，要怎么申请呢？ 是在宿主工程中申请吗？
    * 要在宿主都注册
13. Service的操作是在主线程中运行的，可以考虑用intentservices。四大组件都是运行在主线程的
14. Github上有哪些用组件化的开源项目，从0到1详细讲解的那种？
    * https://juejin.im/post/5b5f17976fb9a04fa775658d
    * https://github.com/guiying712/AndroidModulePattern，这是个组件化的demo
15. 刚哥有比较好的字节码插桩方面的资料吗
    * https://www.jianshu.com/p/16ed4d233fd1
16. 最近代理封了后发现github上下新项目下来如果是以前没下载过的库一直build失败，提示connection refused
    * https://www.jianshu.com/p/dc12c965abbc，很可能是最后一条
17. 定时器一般用什么做
    * 10s左右可以用Timer做，再长一点用alarm或者jobschedule，handler做定时有点不太正规。
    * Rxjava
18.  Flutter只是在android原生中添加一个surfaceview
19. 反编译用什么软件好啊？？
    * jadx
20. **防止截屏**
    *  只需要在 Activity 的onCreate() 方法中添加一行代码即可：getWindow().addFlags(WindowManager.LayoutParams.FLAG_SECURE);
21. 如何将aar 上传到 maven 仓库
    * https://juejin.im/post/5b5dd9e56fb9a04fbd1b3652
22. 对于Flutter集成到现有的项目进行混合开发，你们都是采用哪些方案呢？
    *  Flutter Boost

