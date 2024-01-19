### 构造方法调用
- 子类构造方法默认调用super();
- 
  ps:只能放在第一行（this();也是）
``` java
public class Cat{
    public Cat(String s){
        System.out.println("Cat");
    }
}
public class myCat extends Cat{
    /*
    public myCat(){
        //隐式调用super()而Cat(String s)覆盖了默认无参构造函数从而报错
    }
    */
   /*
   public myCat(){
    Cat();//子类不继承父类构造方法从而报错
   }
   */
  public myCat(){
    super(string);//正确
  }
}
```
- 若调用了this(),则不会再调用super()
- ps:在前一个this()里已经调用了super()

字段声明时可以new而调用某类的构造方法插入本类构造方法的最前端（父类构造方法的后端），而后再调用剩余的本类构造方法（静态static常量final必须初始化，其他的可以暂不初始）

    1.父类字段初始化
    2.父类代码块内实例初始化
    3.父类构造方法
    4.子类字段初始化
    5.子类代码块内实例初始化
    6.子类构造方法
*代码块若位于字段声明前视为临时变量被抛弃*


#### 静态

    静态变量、代码块只在第一次访问该类/对象时加载
    （即在所有初始化最前端，由父即子）
    静态方法不自动加载，使用即加载
    
## 异常
### 异常概念及其层次
异常：预期情况和外部情况
$$
Throwable\{^{Error}_{Exception}
$$

    Error：严重错误(不应尝试捕获或抛出)
    Exception：一般异常（检查异常（捕获重点）/运行时异常（编程时杜绝））
  
### 异常捕获和抛出
#### try-catch-finally
```java
    try{
      //异常语句
    }
    catch(Exception e){
      System.err.println("e.getMessage");//或者直接打印e
      e.printStackTrack();//跟踪，和上一句效果差不多
    }
    finally{
      //有无异常都执行
    }
```
    多个catch挨个捕捉（一般将大类异常置于末尾补漏）
#### throw（方法内抛出）
throw是关键字，抛出异常类到上一级（上一级try-catch块或上一级方法）
```java
throw new Exception(args);//异常类可自己创建,args表示构造方法参数
```
<font color=ffff>throw不在try-catch块中则必须在方法声明里throws</font>

#### throws(方法声明抛出)
```java
(接函数签名)throws Exception{//类名

}
```
上级方法必须响应下级抛出的异常，采用try-catch或throws
### 自定义异常
继承Exception类，重写构造方法
## 多线程
### 线程概述
#### 进程Process
>独立的一次程序执行
#### 线程Thread——CPU调度和分配的基本单位
>进程可以调度多个线程

    进程大量存在共享资源（代码、数据、进程空间等），而线程只独立拥有一些必要资源，如寄存器、栈等。而共享带来了线程之间的竞争。
  ### 线程创建
  >主线程（通常由main方法触发），不比子线程优先级高，且主线程的完结不影响子线程的继续运行
  #### Thread类
  Thread.currentThread()静态方法返回现在线程引用<br>
  Thread.getName()获得线程名字<br>
  >继承Thread类获得新线程类，并覆盖run方法，写入线程执行语句。run作为子线程入口，main作为主线程入口。启动线程时，在main中调用线程对象的start方法（start会调用run）

    线程类也是类，只是能将要做的事在新线程中完成（类似于自创main方法入口）
  #### Runnable接口
  >只有一个run方法要求覆盖，有利于多个对象共享数据

    实现runnable接口、覆盖run方法。
    生成对象作为一个参数并传入一个Thread类，也可传入多个线程类，开启多个线程（也即多个线程拥有同一个实现了接口的类的数据）
    生成Thread类，start线程
```java
new Thread(//匿名线程对象
  new Runnable(){//直接创建有runnable接口的类
    @override
  }
).start();//创建之后立即启动
```
### 线程控制
>新建New-就绪Runnable-运行Running-阻塞Blocked-死亡

    start启动：
        新建->就绪
    得到CPU资源：
        就绪->运行
    yield抑制运行：
        运行->就绪
    stop(Error,Exception，run结束)完成执行：
        运行->死亡
    join,wait,sleep,suspend等待同步锁：
        运行->阻塞
    resume获得同步锁,sleep完成,notify,join插队线程终止：
        阻塞->就绪
    ？？？：
        阻塞->死亡
#### 线程调度模型
抢占式：优先级和时间一起控制<br>分时调度：平均分配时间控制<br>协作调度：不主动退出则一直使用
>Java中抢占：

    高优先级线程进入，原线程退出，就绪
    yield使原线程让步，就绪
    使原线程阻塞
    同步锁未获得（阻塞）
    原线程结束

>join():调用该方法的线程直接插队，运行该方法的线程需等待调用该方法的线程死亡

#### 多线程同步、线程安全
>访问一个对象的数据都是正确的，则称该对象是线程安全的

final常量一定是线程安全的;volatile也是线程安全的：每次访问时强制重新读取共享内存，变化时也强制送入共享内存

    eg:String内容不可变，改变是创建新内容，即线程安全
- 线程同步是一种保证线程安全的重要手段<br>同步锁：在多线程访问某值时，只有一个线程可以操作，直到修改完毕后，其他线程才能进行操作

    同步锁lock flag/监视器monitor，每个对象只有一个同步锁
>引用类型可以加锁，基本类型不用：栈内存对象自用，堆内存多线程共享
#### synchronized同步方法
可修饰代码块、方法<br>对于方法

    保证同一时刻只有一个线程调用此方法，线程调用多个方法时，只有包含synchronized的方法有同步锁保护，普通方法可多线程调用。
    对于实例方法，同步锁就是this。锁对象即包含共享数据的对象，是实现runnable接口的类并送入不同线程
对于代码块
```java
synchronized (/*同步锁对象*/){
  //同步代码块
}
//单独锁需要同步的数据
```

    代码块存在于方法内，缩小同步范围（由整个方法缩小至方法的一部分）。代码块只保护同步部分，其他线程仍然可以调用非同步部分
### 线程通信
#### wait&notify
在同步synchronized场景下进行通信(同步方法或同步代码块内)
>调用wait方法的线程释放同步锁，自己进入wait池阻塞<br>wait中的线程A可以被线程B进行notify从而可以去争夺同步锁

    wait()参数是时间，时间结束后无论是否被唤醒，都会解除wait