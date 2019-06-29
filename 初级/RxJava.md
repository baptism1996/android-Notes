### RxJava

定义：一个实现异步操作的库

* RxJava 的异步实现，是通过一种扩展的观察者模式来实现的。即被观察者订阅观察者

* Observable可以向Observer发出三种事件，Observable 会持有 Observer的引用

  ![52eb2279jw1f2rx46dspqj20gn04qaad](C:\Users\sicilian\AppData\Local\Temp\chrome_drag7668_3755\52eb2279jw1f2rx46dspqj20gn04qaad.jpg)

* 创建观察者（手动创建）

  ```java
  //观察者可为Observer或Subscriber，Subscriber实际是对Observer的拓展，新增了onStart()和unsubscribe()方法
  Observer<String> observer = new Observer<String>() {
  @Override
  public void onNext(String s) {
  Log.d(tag, "Item: " + s);
  }
  
  @Override
  public void onCompleted() {
  Log.d(tag, "Completed!");
  }
  
  @Override
  public void onError(Throwable e) {
  Log.d(tag, "Error!");
  }
  //调用unsubscribe()后，Subscriber 将不再接收事件，即解除引用，避免内存泄漏
  };
  ```

* 创建观察者（自动创建--利用Action）

  ```java 
  //Action0 是 RxJava 的一个接口，它只有一个方法 call()，这个方法是无参无返回值的;Action0同，只不过有参。
  // onNext()
  Action1<String> onNextAction = new Action1<String>() {
  @Override
  public void call(String s) {
  Log.d(tag, s);
  }
  }
  // onError()
  Action1<Throwable> onErrorAction = new Action1<Throwable>() {
  @Override
  public void call(Throwable throwable) {
  // Error handling
  }
  }；
  // onCompleted()
  Action0 onCompletedAction = new Action0() {
  @Override
  public void call() {
  Log.d(tag, "completed");
  }
  };
  
  // 自动创建 Subscriber ，并使用 onNextAction 来定义 onNext()
  observable.subscribe(onNextAction);
  // 自动创建 Subscriber ，并使用 onNextAction 和 onErrorAction 来定义 onNext() 和 onError()
  observable.subscribe(onNextAction, onErrorAction);
  // 自动创建 Subscriber ，并使用 onNextAction、 onErrorAction 和 onCompletedAction 来定义 onNext()、 onError() 和 onCompleted()
  observable.subscribe(onNextAction, onErrorAction, onCompletedAction);
  ```

  

* 创建被观察者

  ```java
  //1.用creat创建
  Observable observable = Observable.create(new Observable.OnSubscribe<String>() {
  @Override
  public void call(Subscriber<? super String> subscriber) {
  subscriber.onNext("Hello");
  subscriber.onNext("Hi");
  subscriber.onNext("Aloha");
  subscriber.onCompleted();
  }
  });
  //2.用just创建
  Observable observable = Observable.just("Hello", "Hi", "Aloha");
  //3.用from创建
  String[] words = {"Hello", "Hi", "Aloha"};
  Observable observable = Observable.from(words);
  ```

* Observable.subscribe(Subscriber) 的内部实现

  ```java
  //即Observable调用subscribe时，用Subscriber作为参数，并返回一个Subscriber
  public Subscription subscribe(Subscriber subscriber) {
  subscriber.onStart();
  onSubscribe.call(subscriber);
  return subscriber;
  }
  ```

* 在RxJava 事件的发出和消费都是在同一个线程,而要实现异步，需要用到 `Scheduler`.有Schedulers.newThread()，Schedulers.io(),Schedulers.computation()，AndroidSchedulers.mainThread()这几种

  ```java
  
  //被创建的事件的内容 1、2、3、4 将会在 IO 线程发出；
  Observable.just(1, 2, 3, 4)
  .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
  .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程,即打印将发生在主线程
  .subscribe(new Action1<Integer>() {
  @Override
  public void call(Integer number) {
  Log.d(tag, "number:" + number);
  }
  });
  ```

* 变换

  * 大多数人说『RxJava 真是太好用了』的最大原因

  * 将事件序列中的对象或整个序列进行加工处理，**转换**成不同的事件或事件序列,核心方法`lift(Operator)`

  * 在 `Observable` 执行了 `lift(Operator)` 方法之后，会返回一个新的 `Observable`，这个新的 `Observable` 会像一个代理一样，负责接收原始的 `Observable` 发出的事件，并在处理后发送给 `Subscriber`。如图

    ![52eb2279jw1f2rxcrna27j20h40d1q4f](C:\Users\sicilian\AppData\Local\Temp\chrome_drag7668_24577\52eb2279jw1f2rxcrna27j20h40d1q4f.jpg)

  * 有map（），flatMap等API

    * map()

      ```java
      //这里出现了一个叫做 Func1 的类。它和 Action1 非常相似，也是 RxJava 的一个接口,区别在 FuncX 包装的是有返回值的方法。
      Observable.just("images/logo.png") // 输入类型 String
      .map(new Func1<String, Bitmap>() {
      @Override
      public Bitmap call(String filePath) { // 参数类型 String
      return getBitmapFromPath(filePath); // 返回类型 Bitmap
      }
      })
      .subscribe(new Action1<Bitmap>() {
      @Override
      public void call(Bitmap bitmap) { // 参数类型 Bitmap
      showBitmap(bitmap);
      }
      });
      ```

    * flatMap()

      ```java
      //和map()不同的是，flatMap() 中返回的是个 Observable 对象
      Student[] students = ...;
      Subscriber<Course> subscriber = new Subscriber<Course>() {
      @Override
      public void onNext(Course course) {
      Log.d(tag, course.getName());
      }
      ...
      };
      //将初始Observable对象铺平后成为多个Observable对象，然后多个对象用统一的路径把事件传递给Subscriber.
      Observable.from(students)
      .flatMap(new Func1<Student, Observable<Course>>() {
      @Override
      public Observable<Course> call(Student student) {
      return Observable.from(student.getCourses());
      }
      })
      .subscribe(subscriber);
      ```

      

      ![52eb2279jw1f2rx4i8da2j20hg0dydgx](C:\Users\sicilian\AppData\Local\Temp\chrome_drag7668_7017\52eb2279jw1f2rx4i8da2j20hg0dydgx.jpg)

