### 设计模式：

​	使用设计模式：

* 复杂度增加；
* 开发成本增加；
* 维护成本降低；

#### 普通工厂模式：

​	可以扩展产品，可以生成新的产品工厂，在产品的维度；

​	缺点：如果生成新的产品系列，会造成工厂泛滥。

#### 抽象工厂模式：

​	不容易增加新的产品品种，但是可以换新的工厂系列。

​	举例：更换软件主题。

#### 桥接模式：

​	在子类的扩展有两个维度，需要解决他们之间的排列组合。

​	举例：跨平台图像浏览系统。一个维度为图像，一个维度为系统

#### 命令模式：

​	将请求发送者和接收者完全解耦，发送者维持对抽象命令的引用，接收者维持对具体实现命令的引用。

​	举例：自定义功能键。

​	缺点：会造成有过多的具体命令类。

#### 策咯模式：

​	提供算法复用机制，用户不需要知道每一种算法的具体实现。

​	举例：电影售票，针对不同群体的不同策略。Ticket类只需要拥有抽象策略的引用，具体实现由子类完成。

​	缺点：用户需要知道有哪些算法或者策略。

##### 命令与策略的区别：

​	两者面向的问题层面是不一样：

​	命令模式强调解耦发送者和接收者以实现不同的命令执行方式；

​	策略模式强调抽象出策略，以实现同种结果的不同实现。

##### 代理与装饰的异同：

​	相同：

​	装饰者模式：装饰者与被装饰者实现同一接口；

​	代理模式：代理类与被代理类实现同一接口；

​	所以两种模式都很容易在真实对象的方法前后加上自定义的方法；

​	区别：

​	装饰器模式关注在一个对象上动态的添加方法，而代理模式关注于		控制对对象的访问，即代理类可以对他的客户隐藏一个对象的具体信息，**他们的目的不同，一个是装饰，一个是限制**。

​	用法：

​	在使用代理模式的时候，我们常常在代理类中创建一个对象的实例；而在装饰者模式的时候，我们通常将原始对象作为一个参数传递给装饰者的构造器（静态代理实现），客户自己指定装饰的是哪一个类。

​	总结：

​	使用代理模式，代理对象和真实对象之间的的关系通常在编译时就已经确定了，而装饰者能够在运行时递归地被构造。

​	举例：

​	A类是原始功能的类， B是装饰模式中对A类的扩展之后的类， C是代理模式中对A类的扩展之后的类

​       对于用户调用来说：
       使用装饰模式，用户更关系的是B的功能(包含A的原始功能)；
       使用代理模式，用户更关心A的功能，并不关心C的功能。

#### 观察者模式：

​	定义对象之间的一种一对多依赖关系。

​	两种类型：推模型/拉模型

​	推模型：主题对象向观察者推送主题的详细信息，不管观察者是否需要，推送的信息通常是主题对象的全部或部分数据。

​	拉模型：拉模型通常都是把主题对象当做参数传递。

​	Subject抽象主题：有增加、删除、通知观察者的方法，还有用来保存所有观察者的集合；

​	Observer抽象观察者：为所有的具体观察者定义一个接口，在得到主题的通知时更新自己。

​	代码：

```java
//抽象主题类
public abstract class Subject {
    /**
     * 用来保存注册的观察者对象
     */
    private    List<Observer> list = new ArrayList<Observer>();
    /**
     * 注册观察者对象
     * @param observer    观察者对象
     */
    public void attach(Observer observer){
        
        list.add(observer);
        System.out.println("Attached an observer");
    }
    /**
     * 删除观察者对象
     * @param observer    观察者对象
     */
    public void detach(Observer observer){
        
        list.remove(observer);
    }
    /**
     * 通知所有注册的观察者对象
     */
    public void nodifyObservers(){
        
        for(Observer observer : list){
            observer.update(this);
        }
    }
}
```

```java
//抽象观察者类
public interface Observer {
    /**
     * 更新接口
     * @param subject 传入主题对象，方面获取相应的主题对象的状态
     */
    public void update(Subject subject);
}
```

```java
//具体主题类
public class ConcreteSubject extends Subject{
    
    private String state;
    
    public String getState() {
        return state;
    }

    public void change(String newState){
        state = newState;
        System.out.println("主题状态为：" + state);
        //状态发生改变，通知各个观察者
        this.nodifyObservers();
    }
}
```

```java
//具体观察者类
public class ConcreteObserver implements Observer {
    //观察者的状态
    private String observerState;
    
    @Override
    public void update(Subject subject) {
        /**
         * 更新观察者的状态，使其与目标的状态保持一致
         */
        observerState = ((ConcreteSubject)subject).getState();
        System.out.println("观察者状态为："+observerState);
    }

}
```

```java
//客户端类
public class Client {

    public static void main(String[] args) {
        //创建主题对象
        ConcreteSubject subject = new ConcreteSubject();
        //创建观察者对象
        Observer observer = new ConcreteObserver();
        //将观察者对象登记到主题对象上
        subject.attach(observer);
        //改变主题对象的状态
        subject.change("new state");
    }

}
```

