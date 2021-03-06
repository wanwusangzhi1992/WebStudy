## FactoryMethod

### 模式的定义与特点
工厂方法（FactoryMethod）模式的定义：定义一个创建产品对象的工厂接口，将产品对象的实际创建工作推迟到具体子工厂类当中。这满足创建型模式中所要求的“创建与使用相分离”的特点。 

工厂方法模式的主要优点有：
+ 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程；
+ 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则；

其缺点是：
+ 每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类，这增加了系统的复杂度

### JS实现
```

<script type="text/javascript">
	/**
	* 简单在厂模式，适用小型权限项目
	*/
	// 简单工厂
	var UserFactory = function(role) {
		function SuperAdmin() {
			this.name = '超管'
			this.roles = ['create', 'retrieve', 'delete', 'update']
		}
		function Admin() {
			this.name = '管理员'
			this.roles = ['retrieve', 'delete', 'update']
		}
		function User() {
			this.name = '用户'
			this.roles = ['retrieve', 'update']
		}
		switch (role) {
			case 'SuperAdmin': return new SuperAdmin(); break;
			case 'Admin': return new Admin(); break;
			case 'User': return new User(); break;
		}
	}
	var spAdmin = UserFactory('SuperAdmin')
	var admin = UserFactory('Admin')
	var user = UserFactory('User')
	console.log('spAdmin', spAdmin)
	console.log('admin', admin)
	console.log('user', user)

	/**
	* 工厂模式：优化上述模式，增加扩展
	*/
	var UserFactory2 = function(role) {
		// 这里是关键点，通过函数调用，然后返回实例化的UserFactory2对象
		// 当对象再次调用时，this instanceof UserFactory2 即为true
		// 返回原型链中实例化的函数
		if (this instanceof UserFactory2) {
			return new this[role]()
		}
		return new UserFactory2(role)
	}
	UserFactory2.prototype = {
		SuperAdmin: function() {
			this.name = '超管'
			this.roles = ['create', 'retrieve', 'delete', 'update']
		},
		Admin: function() {
			this.name = '管理员'
			this.roles = ['retrieve', 'delete', 'update']
		},
		User: function() {
			this.name = '用户'
			this.roles = ['retrieve', 'update']
		}
	}
	var spAdmin = UserFactory2('SuperAdmin')
	var admin = UserFactory2('Admin')
	var user = UserFactory2('User')
	console.log('spAdmin', spAdmin)
	console.log('admin', admin)
	console.log('user', user)

	/**
	* 工厂模式
	*/
	var ShapeFactory = function() {
	}
	ShapeFactory.prototype = {
		create: function(role) {
			if (this[role]) {
				return new this[role]()
			}
		},
		Circle: function() {
			console.log('this', this)
			this.draw = function() {
				console.log('draw Circle')
			}
		},
		Rectangle: function() {
			this.draw = function() {
				console.log('draw Rectangle')
			}
		},
		Square: function() {
			this.draw = function() {
				console.log('draw Square')
			}
		}
	}

	var absFactory = new ShapeFactory()
	var circle = absFactory.create('Circle')
	circle.draw() // draw Circle
	var rectangle = absFactory.create('Rectangle')
	rectangle.draw() // draw rectangle
	var square = absFactory.create('Square')
	square.draw() // draw square
	// var square1 = absFactory.create('Square1')
	// square1.draw() // 执行会报错了

	// 通过上一步
	// square1.draw() // 执行会报错了
	// 可以进一步封装成抽象模式，当不存在时，调用抽象类的方法，提示需要重写方法。
	/**
	* 抽象工厂模式
	*/
	// 定义抽象类，还有抽象方法
	var AbStractFactor = function() {}
	AbStractFactor.prototype = {
		draw: function() {
			console.log('未实现抽象方法或抽象类')
		}
	}
	var AbstractShapeFactory = function() {
		// 正常抽象方法是不能实例化的，可以通过函数再次进行类型赋值
		this.abstractFactor = new AbStractFactor()
	}
	AbstractShapeFactory.prototype = {
		create: function(role) {
			if (this[role]) {
				return new this[role]()
			}
			return this.abstractFactor
		},
		Circle: function() {
			this.draw = function() {
				console.log('draw Circle')
			}
		},
		Rectangle: function() {
			this.draw = function() {
				console.log('draw Rectangle')
			}
		},
		Square: function() {
			this.draw = function() {
				console.log('draw Square')
			}
		}
	}
	

	var absFactory = new AbstractShapeFactory()
	var circle = absFactory.create('Circle')
	circle.draw() // draw Circle
	var rectangle = absFactory.create('Rectangle')
	rectangle.draw() // draw rectangle
	var square = absFactory.create('Square')
	square.draw() // draw square
	var square1 = absFactory.create('Square1')
	square1.draw() // 报错了~~~



</script>



```