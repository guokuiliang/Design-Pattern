<h1> <font color=pink>访问者模式(Visitor Pattern)</font></h1>

<h2> 分类：对象行为型模式</h2>

<h2> 关键字:  Visitor  Element  ObjectStructure	   double-dispatch(双重分派)</h2>

定义：表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

适用性：
1.一个对象结构包含很多类对象。它们有不同的接口，而你想对这些对象实施一些依赖于其具体类的操作。
2.需要对一个对象结构中的对象进行很多不同的并且不相关的操作。而你想避免让这些操作污染这些对象的类。访问者模式使得你可以将相关的操作集中起来定义在一个类中。当该对象结构被很多应用共享时
3.定义对象结构的类很少改变，但经常需要在此结构上定义新的操作。改变对象结构类需要重新定义所有访问者的接口。可能需要很大的代价。如果对象结构类经常改变，那么可能还是在这些类中定义这些操作较好。

现实中的例子：
1.去医院医生开完处方单。划价人员拿着处方单之后根据药品名称和数量计算总价。药房的工作人员则根据药品名称和数量准备药品。

优点：
1.增加新的访问操作很方便。使用访问者模式，增加新的访问操作就意味着增加一个新的具体访问者类，实现简单，无须修改源代码，符合“开闭原则”。
2.将有关元素对象的访问行为集中到一个访问者对象中，而不是分散在一个个的元素类中。类的职责更加清晰，有利于对象结构中元素对象的复用，相同的对象结构可以供多个不同的访问者访问。
3.让用户能够在不修改现有元素类层次结构的情况下，定义作用于该层次结构的操作。

缺点：
1.增加新的元素类很困难。在访问者模式中，每增加一个新的元素类都意味着要在抽象访问者角色中增加一个新的抽象操作，并在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”的要求。
2.破坏封装。访问者模式要求访问者对象访问并调用每一个元素对象的操作，这意味着元素对象有时候必须暴露一些自己的内部操作和内部状态，否则无法供访问者访问。

PS
访问者模式对于增加新的访问操作很方便，但是对于增加新的元素类则很困难。这就是访问者模式的开闭原则的倾斜性。访问者模式适用于数据结构相对稳定的系统。它把数据结构和作用于结构上的操作之间的耦合解开。使得操作集合可以相对自由的演化。访问者模式可以和组合模式一起使用。
一般在对一个集合元素要进行多种操作。并且有可能当还会加入新的对集合元素的操作时可以用访问者模式。访问者模式也是比较公认的最难理解的模式。但是我觉得最难理解的应该是解释器模式中的语法分析。- -！

扩展

分派：
变量被声明时的类型叫做变量的静态类型(Static Type) 又叫明显类型(Apparent Type)。
变量所引用的对象的真实类型又叫做变量的实际类型(Actual Type)。
根据对象的类型而对方法进行的选择,就是分派(Dispatch)。
根据分派发生的时期，可以将分派分为两种，即分派分静态分派和动态分派。
静态分派(Static Dispatch) 发生在编译时期，分派根据静态类型信息发生。方法重载(Overload)就是静态分派。（所谓的：编译时多态）
动态分派(Dynamic Dispatch) 发生在运行时期，动态分派动态地置换掉某个方法。面向对象的语言利用动态分派来实现方法置换产生的多态性。（所谓的：运行时多态）
一个方法所属的对象叫做方法的接收者，方法的接收者与方法的参量统称做方法的宗量。
一个方法根据两个宗量的类型来决定执行不同的代码，这就是“双分派”或者“多重分派”。Java不支持动态的多分派。但可以通过使用设计模式，在Java语言里面实现动态的双重分派（ps：就是“伪双重分派”是由两次的单分派组成）
更详细的看下面这位同学写的吧。
https://www.cnblogs.com/youxin/archive/2013/05/25/3099016.html
访问者模式通过元素的继承动态的分派一次，然后根据传入访问者参数又静态的分派一次。所以访问者是双重分派

参与者：
●Vistor（抽象访问者）：
--抽象访问者为对象结构中每一个具体元素类ConcreteElement声明一个访问操作
--从这个操作的名称或参数类型可以清楚知道需要访问的具体元素的类型
●ConcreteVisitor（具体访问者）：
--具体访问者实现了每个由抽象访问者声明的操作
--每一个操作用于访问对象结构中一种类型的元素
●Element（抽象元素）：
--抽象元素一般是抽象类或者接口，
--它定义一个accept()方法，该方法通常以一个抽象访问者作为参数。
●ConcreteElement（具体元素）：
--具体元素实现了accept()方法，在accept()方法中调用访问者的访问方法以便完成对一个元素的操作。
● ObjectStructure（对象结构）：
--对象结构是一个元素的集合，它用于存放元素对象，并且提供了遍历其内部元素的方法。
--它可以结合组合模式来实现，也可以是一个简单的集合对象，如一个List对象或一个Set对象。

类图：


核心代码：



```java
//某果零售店为庆祝成立30周年，将店内所有产品全部进行促销活动，其中包括(phone = 5000元、pad = 3000元、mac = 9000元)所有商品九折出售。
//现在请按照以上需求编写一个商品计价器。
//这个问题中，由于打折扣是一个可变的因素，所以这里要将打折的算法进行抽象

/**
 * Created by quinn on 26/05/2018.
 */
abstract class Production {
    private String name;
    private double price;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}

class Phone extends Production {
    public Phone() {
        setName("phone");
        setPrice(5000);
    }
}

class Pad extends Production {
    public Pad() {
        setName("pad");
        setPrice(3000);
    }
}

class Mac extends Production {
    public Mac() {
        setName("mac");
        setPrice(9000);
    }
}

abstract class Strategy {
    abstract double discount(double price);
}

class TenPercentDiscount extends Strategy {
    @Override
    double discount(double price) {
        return price * 0.9;
    }
}

class TwentyPercentDiscount extends Strategy {
    @Override
    double discount(double price) {
        return price * 0.8;
    }
}

class Context{
    //可以通过配置文件的方式替换策略
    private Strategy st = new TenPercentDiscount();
    public void checkOut(List<Production> list){
        double count = 0;
        for (Production production:list){
            count = count + st.discount(production.getPrice());
        }
        System.out.println("总价为：" + count);
    }
}

class StrategyDemo{
    public static void main(String[] args) {
        List<Production> list = Lists.newArrayList();
        list.add(new Phone());
        list.add(new Pad());
        list.add(new Mac());
        Context ctx = new Context();
        ctx.checkOut(list); 
    }
}
//策略模式无法解决程序到底选择哪个策略。只是将每个策略独立出来。通过配置文件的方式或者其他方式进行手动指定。避免过于复杂的if语句。但是策略模式无法串行的执行多个策略。也就是说客户端每次只能使用一个策略类。
```
```java
//某果零售店为庆祝成立30周年，将店内所有产品全部进行促销活动，其中包括(phone = 5000元、pad = 3000元、mac = 9000元)所有商品九折出售。由于销售情况良好。店家决定采取折上折的方式在九折的基础上继续再打八折
//现在请按照以上需求编写一个商品计价器。
//折上折，第一种做法可以新增一个策略类。实现一个先打八折又打九折的情况。但这个做法如果在折扣经常会发生组合改变的情况下。可能子类会产生笛卡尔积。就是100个子类。为了避免这种情况的方法。现在重构策略类
//把折扣类当作一个具体构件类Component，又为装饰类Decorator,折上折可以理解为折扣1装饰折扣2.

class DecorateDemo {
    public static void main(String[] args) {
        List<Production> list = Lists.newArrayList();
        list.add(new Phone());
        list.add(new Pad());
        list.add(new Mac());
        //可以通过配置文件对方式注入替换策略，
        NewStrategy st = new NewTenPercentDiscount(new NewTwentyPercentDiscount());
        double count = 0;
        for (Production production : list) {
            count = count + st.discount(production.getPrice());
        }
        System.out.println("总价为：" + count);
    }
}

abstract class NewStrategy {
    private NewStrategy strategy;
    public NewStrategy() {
    }
    public NewStrategy(NewStrategy strategy) {
        this.strategy = strategy;
    }
    public double discount(double price) {
        if (strategy != null) {
            price = strategy.discount(price);
        }
        return price;
    }
}

class NewTenPercentDiscount extends NewStrategy {
    public NewTenPercentDiscount() {
    }
    public NewTenPercentDiscount(NewStrategy strategy) {
        super(strategy);
    }
    @Override
    public double discount(double price) {
        price = super.discount(price);
        return price * 0.9;
    }
}

class NewTwentyPercentDiscount extends NewStrategy {
    public NewTwentyPercentDiscount() {
    }
    public NewTwentyPercentDiscount(NewStrategy strategy) {
        super(strategy);
    }
    @Override
    public double discount(double price) {
        price = super.discount(price);
        return price * 0.8;
    }
}
//使用装饰者得方式解决这个问题，避免了子类爆炸的问题，并且可以使客户端同时执行多个策略。但是缺点在于在创建策略时会变得特别的繁琐。
```

```java
//某果零售店为庆祝成立30周年，将店内所有产品全部进行促销活动，其中包括(phone = 5000元、pad = 3000元、mac = 9000元)全场phone一律八折，全场Mac一律7折，全场pad一律九折
//现在请按照以上需求编写一个商品计价器。
//考虑到固定的产品会有固定的折扣。所以这里可以将手动指定策略的方法改为由一个简单工厂动态选择策略。在简单工厂类中有个Map来存放产品名与对应折扣策略。（这部分可以指定从数据库读或者从配置文件读取）
```
