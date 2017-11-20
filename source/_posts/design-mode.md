---
title: 浅析设计模式在web开发中的应用<一>：单一职责
date: 2017-09-25 11:13:05
categories: web开发
tags: [设计模式, php]
---
经过接近两年的前端知识的学习，多多少少积累了不少知识和经验。一直想着写点东西，由于时间和自己学艺不精的原因，没有太多心思去写博客，现在暂时有点闲功夫了。想着还是写一点东西吧，所以这篇博客出来了。

### 单一职责（SRP）
#### 1.简单举个例子
一家 2000 人的工厂生产出 1000 辆汽车，如果每 2 个人负责一辆汽车的制造，要完成这个生产任务，可能几十年都完成不了。但是如果把这个任务拆分成一个一个小的任务，这里的每个工人都有自己独有的任务，有着专业的技能，那么每个人的职责是单一的，制作轮子的不必管制作灯泡的。这样效率就大大的提升了。是不是很容易理解呢

#### 2.在敏捷软件开发中，把“职责”定义为“变化的原因”
对于一个类而言，应该只有一个引起它变化的原因，这句话怎么理解呢？
简单的说：就是一个类只负责一个功能领域里的一个职责，不同变化的原因放在不同的类中。
比如说 MVC 框架中，表单插入数据库字段过滤与安全检查应该放在 controller 层还是 model 层？
数据库过滤与安全检查是表单插入这一个功能领域里的一个职责，它应该单独封装成类，所以应该放在 model 层。
从上面的描述中可以看出，单一职责有 2 层含义：第一是避免相同的职责分散到不同的类中，第二避免一个类承担了太多的职责。

#### 3.单一职责的好处在于:
- 减少类之间的耦合：需求发生变化时，只修改一个类从而隔离了变化。如果一个类有不同的职责，当这些职责都耦合在一起，当一个职责发生变化时就会影响其他的职责。
- 提高类的复用性：当需要修改某个职责时，只需要替换掉这个类，不会影响其他的类的职责。
现在流行的组件化开发，就是使用了单一职责这一模式。

#### 4.单一职责的体现
(1)工厂模式：负责生产对象，提供不同的参数生产不同的对象
```php
<?php
    interface Db_Adapter {
        public function connect($config) {
            public function query($query, $handle);
        }
    }
    // 抽象的接口，并未给出具体的实现
?>
```

```php
<?php
    class Db_Adapter_Mysql implements Db_Adapter {
        private $_dbLink;
        public function connect($config) {
            /** code */
        }
    }
    // MySQL的操作类，也可以是SQLite或者Oracle等等
?>
```

```php
<?php
    class sqlFactory {
        public static function factory($type) {
            if(include_once 'Drivers/'.$type.'.php') {
                $classname = 'Db_Adapter_'.$type;
                return new $classname;
            } else {
                throw new Exception('Driver not found');
            }
        }
    }
    // 工厂模式，生成不同的对象
?>
```
__思考：工厂模式与单一职责有什么关系呢？__
其实有很大关系的，从上面我们可以看出，MySQL 操作，还有 SQLite 等等数据库的操作，都是用单独的类写出来的，这样做的原因就是，避免这些职责耦合到一个功能类里，所以把这些职责用单独的类封装起来了。但是单独封装了就会有个问题，一旦产生了功能变化，就应该可以自由的切换到各自的类中生成不同的对象。所以使用工厂模式来实现这一性质，当功能发生变化时，在执行的时候，传入恰当的参数就可以切换到相应的类生成相应的对象了。

(2)命令模式
命令模式将“命令请求者”和“命令执行者”职责分开，生成单独的职责。
举个例子，你去餐馆吃饭，餐馆有员工，厨师等角色，作为顾客，你需要把菜单投递给员工，由员工去通知厨师主管去实现，厨师收到菜单后，就按照菜单上的内容开始做菜。这里命令的请求和实现就完成了解耦。
代码模拟这一过程
```php
<?php
// 模拟厨师
class cooker {
    public function meal() {
        echo 'meal';
    }
    public function drink() {
        echo 'drink';
    }
    public function over() {
        echo 'ok';
    }
    interface Command {
        public function execute();
    }
}

?>
```

```php
<?php
// 模拟员工和厨师,绑定命令接受者
class MealCommand implements Command {
    public function __construct(cooker $cooker) {
        $this->cooker = $cooker;
    }
    public function execute() {
        $this->cooker->meal();
    }
}
?>
class DrinkCommand implements Command {
    public function __construct(cooker $cooker) {
        $this->cooker = $cooker;
    }
    public function execute() {
        $this->cooker->drink();
    }
}
?>
```

```php
// 模拟员工与顾客
class cookerControl {
    private $mealcommand;
    private $drinkcommand;
    public function addCommand(Command $mealcommand, Command $drinkcommand) {
        $this->mealcommand = $mealcommand;
        $this->drinkcommand = $drinkcommand;
    }
    public function calldrink() {
        $this->mealcommand->execute();
    }
    public function calldrink() {
        $this->drinkcommand->exexute();
    }
}
```

```php
// 实现命令模式
$control = new cookControl;
$cooker = new cooker;
$mealcommand = new MealCommand($cooker);
$drinkcommand = new DrinkCommand($cooker);
$control->addCommand($mealCommand, $drinkcommand);
$control->callmeal();
$control->calldrink();
```
代码模拟出来了，为啥要这么写呢?当然是解耦啦，但这是怎么个解耦法呢？这样做有好处呢？
由上可以看出，第一个好处，厨师的做饭和做喝的是分离的，是两个不同的类，做饭这一职责和做菜这一职责相互独立互不影响。第二个好处，顾客的命令和厨师的执行是分离的，一旦厨师的执行出现了问题，重新执行就可以了，但是如果顾客未与厨师功能分离开来，这整个命令的过程都会受到影响。
(3)MVC 模式
![img](/images/dm1.png)
由上图可知，这些业务是非常复杂的，各层之间分层很细，有很多子模块，系统的总体设计的原则是，把复杂的业务逻辑分成各种子模块，子系统。这样的架构更加利用，各个模块之间的解耦和分工合作。