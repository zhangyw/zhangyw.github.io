# 创建对象
-----
## 工厂模式
```javascript
	function createPerson(name, age, job) {
		var obj = new Object();
		obj.name = name;
		obj.age = age;
		obj.job = job;
		obj.sayName = function(){
			alert(this.name);
		};
		return obj;
	}
```    
> 工厂模式的缺点是没有解决对象识别的问题，无法知道对象的类型


## 构造函数模式
```
	function Person(name,age,job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
			alert(this.name);
		};
	}
    var person1 = new Person("Jacky",20,"Software Engineer");
```
这种方式创建新实例，必须使用new，实际上会经历4个步骤
* 创建一个新对象 new Object()
* 将构造函数的作用域赋值给新建的对象，this指向这个新对象了
* 执行构造函数中的代码，给新对象增加属性
* 返回新对象
> 构造函数模式的缺点是：如果在构造函数内有function属性，如上例中的sayName，这样每个实例都要创建一个实现了同样功能的function对象  
> <pre> person1.sayName == person2.sayName ; //false </pre>
> 如果在全局作用域中定义sayName，又完全没有封装可言。


## 原型模式
```
// 原型模式
	function Person(){}
	Person.prototype = {
		name:'Jacky',
		age: 23,
		job: 'Teacher',
                address:['江苏','南京'],
		sayName: function(){
			console.log(this);
			alert(this.name);
		}
	}
```
> 原型模式的缺点是所有属性都共享的。例如修改了new Person().address.push('yuhuatai')。所有的实例的address的值都会修改。这不符合一般实例需要独有的属性的需求

## 组合使用构造函数模式和原型模式
> 创建自定义类型的**最常见**方式就是使用__组合使用构造函数模式和原型模式__。构造函数模式用于一定实例属性，而原型模式用于定义方法和共享属性。

```
	function Person(name,age,job){
		this.name = name;
		this.age = age;
		this.job = job;
	}
	Person.prototype ={
		constructor:Person,
		sayName:function(){
			alert(this.name);
		}
	}

```


## 动态原型模式

```
	function Person(name,age,job){
		this.name = name;
		this.age = age;
		this.job = job;
		if((typeof Person.prototype.sayName) != 'function'){
			Person.prototype.sayName = function(){
				alert(this.name);
			}
		}
	}
```
在组合构造函数模式和原型模式的基础上，把原型封装到构造函数里，在第一次调用构造函数的时候初始化

## 寄生构造函数模式
```
	function SpecialArray(){
		var values = new Array();
		values.push(arguments);
		values.toPipedString = function(){
			return this.join('|')
		}
		return values;
	}
    var sarray1 = new SpecialArray('aa','bb','cc');
```
> 如上例，如果想要创建一个具有额外方法的数组，又不能直接修改Array的构造方法。可以使用上面的方式来创建
> 这种方式创建的对象无法使用instanceof操作符来确定类型，不推荐使用。
可以使用下面的组合构造函数和原型的模式方式实现同样的效果

```
	function SpecialArray(){
		this.push.apply(this,arguments);
		this.toPipedString = function(){
		    return this.join('|')
		}
	}

	SpecialArray.prototype = Array.prototype;

```
> 这种方式的优势是可以使用instanceof 检测实例的类型。所以还不知道寄生构造函数的作用是什么？？



## 稳妥构造函数模式
```
	function Person(name,age,job){
		var o = new Object();
		o.sayName = function(){
			alert(name);
		}
		return o;
	}

	var person =  Person('jacky',20,'worker');

```
> 稳妥对象：没有公共属性






















































































































