---
layout:     post
title:      "PHP演绎设计模式-创建型模式"
subtitle:   " 工厂模式，抽象工厂模式，单例模式，建造者模式，原型模式"
date:       2017-07-12 14:16:05
author:     "cuizhazha"
header-img: ""
header-bg-css: "linear-gradient(to right, #24b94a, #38ef7d);"
catalog: true
tags:
    - PHP
    - 设计模式
    - 创建型模式
---
# 工厂模式

工厂模式（Factory Pattern）是最常用的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

#介绍

**意图：**定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。**主要解决：**主要解决接口选择的问题。

**何时使用：**我们明确地计划不同条件下创建不同实例时。

**如何解决：**让其子类实现工厂接口，返回的也是一个抽象的产品。

**关键代码：**创建过程在其子类执行。

**应用实例：**

~~~
1、您需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。 

2、切换数据库只需换驱动即可。

~~~

**优点：**

~~~
1、一个调用者想创建一个对象，只要知道其名称就可以了。 

2、扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。 

3、屏蔽产品的具体实现，调用者只关心产品的接口。

~~~

**缺点：**每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

**使用场景：**

~~~
1、日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方。 

2、数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 

3、设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口。

~~~

**注意事项：**作为一种创建类模式，在任何需要生成复杂对象的地方，都可以使用工厂方法模式。有一点需要注意的地方就是复杂对象适合使用工厂模式，而简单对象，特别是只需要通过 new 就可以完成创建的对象，无需使用工厂模式。如果使用工厂模式，就需要引入一个工厂类，会增加系统的复杂度。

#例子

步骤1 创建一个接口

~~~
interface Shape {
   public function draw();
}

~~~

步骤2 创建实现接口的实体类

~~~
class Rectangle implements Shape {
   public function draw() {
      echo "Inside Rectangle::draw() method.";
   }
}

class Square implements Shape {
   public function draw() {
      echo "Inside Square::draw() method.";
   }
}

class Circle implements Shape {
   public function draw() {
      echo "Inside Circle::draw() method.";
   }
}

~~~

步骤3 创建一个工厂，生成基于给定信息的实体类的对象

~~~
public class ShapeFactory {
    public function getShape(shapeType) {
      if ('CIRCLE' == shapeType) {
          return new Circle();
      } elseif ('RECTANGLE' == shapeType) {
          return new Rectangle();
      } elseif ('SQUARE' == shapeType) {
          return new Square();
      }
    }
}

~~~

步骤4 使用该工厂，通过传递类型信息来获取实体类的对象

~~~
$shapeFactory = new ShapeFactory();
//获取 Circle 的对象，并调用它的 draw 方法
$shape1 = $shapeFactory->getShape("CIRCLE");
//调用 Circle 的 draw 方法
$shape1->draw();

//获取 Rectangle 的对象，并调用它的 draw 方法
$shape2 = $shapeFactory->getShape("RECTANGLE");
//调用 Rectangle 的 draw 方法
$shape2->draw();

//获取 Square 的对象，并调用它的 draw 方法
$hape3 = $shapeFactory->getShape("SQUARE");
//调用 Square 的 draw 方法
$shape3->draw();
~~~
# 抽象工厂模式

抽象工厂模式（Abstract Factory Pattern）是围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

在抽象工厂模式中，接口是负责创建一个相关对象的工厂，不需要显式指定它们的类。每个生成的工厂都能按照工厂模式提供对象。

#介绍

**意图：**提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

**主要解决：**主要解决接口选择的问题。

**何时使用：**系统的产品有多于一个的产品族，而系统只消费其中某一族的产品。

**如何解决：**在一个产品族里面，定义多个产品。

**关键代码：**在一个工厂里聚合多个同类产品。

**应用实例：**工作了，为了参加一些聚会，肯定有两套或多套衣服吧，比如说有商务装（成套，一系列具体产品）、时尚装（成套，一系列具体产品），甚至对于一个家庭来说，可能有商务女装、商务男装、时尚女装、时尚男装，这些也都是成套的，即一系列具体产品。假设一种情况（现实中是不存在的，要不然，没法进入共产主义了，但有利于说明抽象工厂模式），在您的家中，某一个衣柜（具体工厂）只能存放某一种这样的衣服（成套，一系列具体产品），每次拿这种成套的衣服时也自然要从这个衣柜中取出了。用 OO 的思想去理解，所有的衣柜（具体工厂）都是衣柜类的（抽象工厂）某一个，而每一件成套的衣服又包括具体的上衣（某一具体产品），裤子（某一具体产品），这些具体的上衣其实也都是上衣（抽象产品），具体的裤子也都是裤子（另一个抽象产品）。

**优点：**当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。

**缺点：**产品族扩展非常困难，要增加一个系列的某一产品，既要在抽象的 Creator 里加代码，又要在具体的里面加代码。

**使用场景：**

~~~
1、QQ 换皮肤，一整套一起换。 

2、生成不同操作系统的程序。

~~~

**注意事项：**产品族难扩展，产品等级易扩展。

#例子

步骤1 创建一个接口

~~~
interface Shape {
   public function draw();
}

~~~

步骤2 创建实现接口的实体类

~~~
class Rectangle implements Shape {
   public function draw() {
      echo "Inside Rectangle::draw() method.";
   }
}

class Square implements Shape {
   public function draw() {
      echo "Inside Square::draw() method.";
   }
}

class Circle implements Shape {
   public function draw() {
      echo "Inside Circle::draw() method.";
   }
}

~~~

步骤3 为 Shape 对象创建抽象类来获取工厂

~~~
abstract class AbstractFactory {
   public abstract function getShape(String shape);
}

~~~

步骤4 创建一个工厂，生成基于给定信息的实体类的对象

~~~
public class ShapeFactory extends AbstractFactory {
    public function getShape(shapeType) {
      if ('CIRCLE' == shapeType) {
          return new Circle();
      } elseif ('RECTANGLE' == shapeType) {
          return new Rectangle();
      } elseif ('SQUARE' == shapeType) {
          return new Square();
      }
    }
}

~~~

步骤 5 创建一个工厂创造器/生成器类，通过传递形状或颜色信息来获取工厂

~~~
class FactoryProducer {
   public static function getFactory(String choice){
      if('SHAPE' == choice){
         return new ShapeFactory();
      }

      return null;
   }
}

~~~

步骤6 使用该工厂，通过传递类型信息来获取实体类的对象

~~~
$shapeFactory = FactoryProducer.getFactory("SHAPE")
//获取 Circle 的对象，并调用它的 draw 方法
$shape1 = $shapeFactory->getShape("CIRCLE");
//调用 Circle 的 draw 方法
$shape1->draw();

//获取 Rectangle 的对象，并调用它的 draw 方法
$shape2 = $shapeFactory->getShape("RECTANGLE");
//调用 Rectangle 的 draw 方法
$shape2->draw();

//获取 Square 的对象，并调用它的 draw 方法
$hape3 = $shapeFactory->getShape("SQUARE");
//调用 Square 的 draw 方法
$shape3->draw();
~~~
# 单例模式

单例模式（Singleton Pattern）是最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

**注意：**

~~~
1、单例类只能有一个实例。

2、单例类必须自己创建自己的唯一实例。

3、单例类必须给所有其他对象提供这一实例。

~~~

#介绍

**意图：**保证一个类仅有一个实例，并提供一个访问它的全局访问点。

**主要解决：**一个全局使用的类频繁地创建与销毁。

**何时使用：**当您想控制实例数目，节省系统资源的时候。

**如何解决：**判断系统是否已经有这个单例，如果有则返回，如果没有则创建。

**关键代码：**构造函数是私有的。

**应用实例：**

~~~
1、一个班级只有一个班主任。

2、Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。

3、一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。

~~~

**优点：**

~~~
1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。

2、避免对资源的多重占用（比如写文件操作）。

~~~

**缺点：**没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

**使用场景：**

~~~
1、要求生产唯一序列号。

2、WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。

3、创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。

~~~

**注意事项：**getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。

#例子

步骤1 创建一个 Singleton 类

~~~
// 单例抽象类，让续承的子类都具有单例模式
abstract class AbstractSingleton
{
    protected static $instances = [];
    protected static $called_class;

    protected function __construct() { }

    public static function getInstance()
    {
        self::$called_class = get_called_class();
        if (!isset(static::$instances[self::$called_class])）{
            static::$instances[self::$called_class] = new static();
        }

        return static::$instances[self::$called_class];
    }

    //私有克隆函数，防止外办克隆对象
    protected function __clone() {}
}

class SingleObject extends AbstractSingleton{
    public function showMessage(){
        echo "Hello World!";
    }
}

~~~

步骤2 从 singleton 类获取唯一的对象

~~~
$object = SingleObject::getInstance();
$object->showMessage();

~~~

步骤3 执行程序，输出结果：Hello World!
# 建造者模式

建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

一个 Builder 类会一步一步构造最终的对象。该 Builder 类是独立于其他对象的。

#介绍

**意图：**将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。

**主要解决：**主要解决在软件系统中，有时候面临着"一个复杂对象"的创建工作，其通常由各个部分的子对象用一定的算法构成；由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是将它们组合在一起的算法却相对稳定。

**何时使用：**一些基本部件不会变，而其组合经常变化的时候。

**如何解决：**将变与不变分离开。

**关键代码：**建造者：创建和提供实例，导演：管理建造出来的实例的依赖关系。

**应用实例：**去肯德基，汉堡、可乐、薯条、炸鸡翅等是不变的，而其组合是经常变化的，生成出所谓的"套餐"。

**优点：**

~~~
1、建造者独立，易扩展。 

2、便于控制细节风险。

~~~

**缺点：**

~~~
1、产品必须有共同点，范围有限制。 

2、如内部变化复杂，会有很多的建造类。

~~~

**使用场景：**

~~~
1、需要生成的对象具有复杂的内部结构。 

2、需要生成的对象内部属性本身相互依赖。

~~~

**注意事项：**与工厂模式的区别是：建造者模式更加关注与零件装配的顺序。

#例子

步骤1 创建一个表示商品列表和包装的接口。

~~~
interface Item {
    public function name();      // 名称
    public function packing();   // 包装
    public function price();     // 价格
}

interface Packing {
    public function pack();
}

~~~

步骤2 创建实现 Packing 接口的实体类

~~~
class Wrapper implements Packing {
    public function pack() {
        return "Wrapper";
    }
}

class Bottle implements Packing {
    public function pack() {
        return "Bottle";
    }
}

~~~

步骤3 创建实现 Item 接口的抽象类，该类提供了默认的功能。

~~~
abstract class Burger implements Item {
    public function packing() {
        return new Wrapper();
    }

    public abstract function price();
}

abstract class Cola implements Item {
    public function packing() {
        return new Bottle();
    }

    public abstract function price();
}

~~~

步骤4 创建扩展了 Burger 和 Cola 的实体类

~~~
class ChickenBurger extends Burger {
    public function price() {
        return 23.5;
    }

    public function name() {
        return "Chicken Burger";
    }
}

class CocaCola extends Cola {
    public function price() {
        return 7;
    }

    public function name() {
        return "CocaCola";
    }
}

class Pepsi extends Cola {
    public function price() {
        return 7.5;
    }

    public function name() {
        return "Pepsi";
    }
}

~~~

步骤5 创建一个 Meal 类，带有上面定义的 Item 对象

~~~
class Meal {
    private $items = [];

    public function addItem($item){
        $this->items[] = $item;
    }

    public function getCost(){
        $cost = 0.0;
      foreach($this->items as $item){
          $cost += $item->price();
      }
      return $cost;
   }

    public function showItems(){
        foreach($this->items as $item){
            var_dump("Item : "  . $item->name());
            var_dump("Packing : " . $item->packing()->pack());
            var_dump("Price : " . $item->price());
        }
    }
}

~~~

步骤6 创建一个 MealBuilder 类，实际的 builder 类负责创建 Meal 对象

~~~
class MealBuilder {
    public function mcdonalds (){
        $meal = new Meal();
        $meal->addItem(new ChickenBurger());
        $meal->addItem(new CocaCola());
        return $meal;
    }

    public function kfc (){
        $meal = new Meal();
        $meal->addItem(new ChickenBurger());
        $meal->addItem(new Pepsi());
        return $meal;
    }
}

~~~

步骤7 BuiderPatternDemo 使用 MealBuider 来演示建造者模式

~~~
$mealBuilder = new MealBuilder();
$mcdonalds = $mealBuilder->mcdonalds();
var_dump("McDonalds");
$mcdonalds->showItems();
var_dump("Total Cost: "  . $mcdonalds->getCost());
echo PHP_EOL;
$kfc = $mealBuilder->kfc();
var_dump("KFC");
$kfc->showItems();
var_dump("Total Cost: " . $kfc->getCost());

~~~

输出的结果：

~~~
string(9) "McDonalds"
string(21) "Item : Chicken Burger"
string(19) ", Packing : Wrapper"
string(14) ", Price : 23.5"
string(15) "Item : CocaCola"
string(18) ", Packing : Bottle"
string(11) ", Price : 7"
string(16) "Total Cost: 30.5"

string(3) "KFC"
string(21) "Item : Chicken Burger"
string(19) ", Packing : Wrapper"
string(14) ", Price : 23.5"
string(12) "Item : Pepsi"
string(18) ", Packing : Bottle"
string(13) ", Price : 7.5"
string(14) "Total Cost: 31"
~~~
# 原型模式

原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。例如，一个对象需要在一个高代价的数据库操作之后被创建。我们可以缓存该对象，在下一个请求时返回它的克隆，在需要的时候更新数据库，以此来减少数据库调用。

#介绍

**意图：**用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

**主要解决：**在运行期建立和删除原型。

**何时使用：**

~~~
1、当一个系统应该独立于它的产品创建，构成和表示时。 

2、当要实例化的类是在运行时刻指定时，例如，通过动态装载。 

3、为了避免创建一个与产品类层次平行的工厂类层次时。 

4、当一个类的实例只能有几个不同状态组合中的一种时。建立相应数目的原型并克隆它们可能比每次用合适的状态手工实例化该类更方便一些。

~~~

**如何解决：**利用已有的一个原型对象，快速地生成和原型对象一样的实例。

**关键代码：**原型模式同样用于隔离类对象的使用者和具体类型（易变类）之间的耦合关系，它同样要求这些"易变类"拥有稳定的接口。

**应用实例：**细胞分裂

**优点：**

~~~
1、性能提高。 

2、逃避构造函数的约束。

~~~

**缺点：**配备克隆方法需要对类的功能进行通盘考虑，这对于全新的类不是很难，但对于已有的类不一定很容易，特别当一个类引用不支持串行化的间接对象，或者引用含有循环结构的时候。

**使用场景：**

~~~
1、资源优化场景。 

2、类初始化需要消化非常多的资源，这个资源包括数据、硬件资源等。 

3、性能和安全要求的场景。 

4、通过 new 产生一个对象需要非常繁琐的数据准备或访问权限，则可以使用原型模式。 5、一个对象多个修改者的场景。 

6、一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用原型模式拷贝多个对象供调用者使用。 

7、在实际项目中，原型模式很少单独出现，一般是和工厂方法模式一起出现，通过 clone 的方法创建一个对象，然后由工厂方法提供给调用者。

~~~

**注意事项：**与通过对一个类进行实例化来构造新对象不同的是，原型模式是通过拷贝一个现有对象生成新对象的。浅拷贝实现 Cloneable，重写，深拷贝是通过实现 Serializable 读取二进制流。

#例子

步骤1 创建一个实现了 Cloneable 接口的抽象类。

~~~
abstract class Shape {
    private $id;
    protected $type;

    public abstract function draw();

    public function getType(){
        return $this->type;
    }

    public function getId() {
        return $this->id;
    }

    public function setId($id) {
        $this->id = $id;
    }
}

~~~

步骤2 创建扩展了上面抽象类的实体类

~~~
class Rectangle extends Shape {
    public function __construct() {
        $this->type = "Rectangle";
    }

    public function draw() {
        echo "Inside Rectangle::draw() method.";
    }
}

class Square extends Shape {

    public function __construct() {
        $this->type = "Square";
    }

    public function draw() {
        echo "Inside Square::draw() method.";
    }
}

class Circle extends Shape {

    public function __construct(){
        $this->type = "Circle";
    }

    public function draw() {
        echo "Inside Circle::draw() method.";
    }
}

~~~

步骤3 创建一个类，将实体类存储在一个 数组 中

~~~
class ShapeCache {

    private static $shapeMap = [];

    public static function getShape($shapeId) {
        $cachedShape = self::$shapeMap[$shapeId];

        return clone $cachedShape;
    }

   // 对每种形状都运行数据库查询，并创建该形状
   // shapeMap.put(shapeKey, shape);
   // 例如，我们要添加三种形状
   public static function loadCache() {
        $circle = new Circle();
        $circle->setId("1");
        self::$shapeMap[$circle->getId()] = $circle;

        $square = new Square();
        $square->setId("2");
        self::$shapeMap[$square->getId()] = $square;

        $rectangle = new Rectangle();
        $rectangle->setId("3");
        self::$shapeMap[$rectangle->getId()] = $rectangle;
   }
}

~~~

步骤4 使用 ShapeCache 类来获取存储在 数组 中的形状的克隆

~~~
ShapeCache::loadCache();

$clonedShape = ShapeCache::getShape("1");
var_dump("Shape : " . $clonedShape->getType());

$clonedShape2 = ShapeCache::getShape("2");
var_dump("Shape : " . $clonedShape2->getType());

$clonedShape3 =  ShapeCache::getShape("3");
var_dump("Shape : " . $clonedShape3->getType());
~~~