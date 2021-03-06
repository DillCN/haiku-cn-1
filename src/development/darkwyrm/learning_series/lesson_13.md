# 第十三课

在上一节中，我们了解了一个编程范式，其围绕对象的设计，构建以及交互来执行一个或多个任务。我们也探究了C++中对象称之为类的原因，对象的组成，以及一些其他的基本内容，但是为了成为一个高效的Haiku开发人员，我们需要学习更多。

## 继承

Haiku 开发人员编写的很多代码都和继承相关，继承即创建一个基于其他类（基类）的类。例如，如果我们拥有一个矩形类（Rectangle），其具有高度（Height），宽度（Width），并且提供了一组用于计算面积（Area）和周长(Perimeter)的函数。创建一个长方体类（RectangularSolid）类也非常容易，仅需添加一个长度（Depth）和一个计算体积（Volume）的函数即可。

如果长方体类的第三个参数是它与长方形的唯一区别，那么就不必要为它们编写用于计算面积和周长的代码。如果我们仅需要为它们不同的地方编写代码，那这就太方便了。继承让这成为了可能：长方体类就称为长方形类的子类。和孩子继承父母的基因一样，子类继承了其父类的属性和方法，而我们的长方体类就继承了长方形类的所有方法和属性。

<table border="1">
<tr> <td>    </td>   <td>长方形	   </td>       <td>长方体</td> </tr>
<tr> <td>属性</td>   <td>Height， Width</td>   <td>Height(继承)，Width(继承), Depth</td> </tr>
<tr> <td>方法</td>   <td>Area，Perimeter</td>  <td>Area(继承)，Perimeter(继承)， Volume</td> </tr>
</table> 

继承允许我们复用已有的代码。代码复用是 C++ 编程的基础之一。再次提醒，work smarter, not harder。

我们将编写一下两个类定义来分别介绍这两个例子：

    class Rectangle
    {
    public:
    			Rectangle(int width, int height);
    		void 	SetWidth(int width);
    		int 	Width(void);
    		void 	SetHeight(int height);
    		int 	Height(void);
    		int 	Area(void);
    		int 	Perimeter(void);
    private:
    		int 	fHeight;
    		int 	fWidth;
    };
     
    // 这行代码说明了 RectangularSolid 是Rectangle的子类。
    class RectangularSolid : public Rectangle
    {
    public:
    			RectangularSolid(int width, int height, int depth);
     
    		// 我们不需要列出从Rectangle继承来的类，
    		// 除了那些新出现的类。
    		void 	SetDepth(int depth);
    		int 	Depth(void);
    		int 	Volume(void);
    private:
    		int	fDepth;
    };

长方体类仅声明了新使用的方法和属性。编写具体的类实现将要比它们的定义需要更多的思考。这是我们第一次编写有关类的代码，所以，别走神，仔细研究一下这些代码和注释。

    // Rectangle的构造函数。它将会将这些值赋给对象的属性。在为
    // 类的方法编写代码时，类名和其后的两个冒号置于方法名之前。
    Rectangle::Rectangle(int width, int height)
    {
    	fWidth = width;
    	fHeight = heigh;
    }
     
    void
    Rectangle::SetWidth(int width)
    {
    	fWidth = width;
    }
     
    int
    Rectangle::Width(void)
    {
    	return fWidth;
    }
     
    void
    Rectangle::SetHeight(int height)
    {
    	fHeight = height;
    }
     
    int
    Rectangle::Height(void)
    {
    	return fHeight;
    }
     
    int 
    Rectangle::Area(void)
    {
    	return fWidth * fHeight;
    }
     
    int
    Rectangle::Perimeter(void)
    {
    	return (2 * fWidth) + (2 * fHeight);
    }
     
    // 下面是RectangularSolid构造函数。在创建RectangularSolid的
    // 同时，它第一次使用Rectangle的构造函数创建了一个Rectangle对象。
    // 我们将跳过height和width，而使用depth初始化fDepth。
    RectangularSolid::RectangularSolid(int width, int height, int depth)
    {
    	fDepth = depth;
    }
     
    Void
    RectangularSolid::SetDepth(int depth)
    {
    	fDepth = depth;
    }
     
    int 
    RectangularSolid::Depth(void)
    {
    	return fDepth;
    }
     
    int
    RectangularSolid::Volume(void)
    {
    	// 我们调用了Width() 和Height() 而不是使用 fWidth 和 fHeight，
    	// 这是因为每个子类都没有访问该对象的私有方法和属性。
    	return Width() * Height() * fDepth;
    }

这段代码和之前我们所写的代码之间的真正不同点在于，对对象的思考强制我们去关心代码的组织。编写紧凑，优雅的代码可以让它的管理和维护非常容易。

在这段代码中，唯一比较陌生的部分莫过于在 Volume() 中调用 Height() 和 Width()。如果子类能够访问 fWidth 和 fHeight，那岂不是更加方便，这就是 protected 的领地了。尽管您可能并不需要或者不会经常用到。

还有一处值得注意的地方是继承的处理，也就是 Rectangle 的 public 部分。这就是继承的类型。通常您所使用的都是公共（public）继承。这也就是说，父类方法和属性的权限不会发生改变。选择其他两种类型将会限制对基类的访问。选择保护（protected）继承将使基类的所有公共属性和方法成为 protected 而非 public，将使它们对所有的子类都可用，但是对外界则不可见。私有（private）继承将使子类所有的属性和方法成为 private，使父类对外界和任何 “grandchild” 完全无法访问。

## 虚函数

子类不仅可以添加新的方法和属性，而且也可以修改已有方法的行为。但是仅当基类允许修改时才行。而在方法声明的返回值类型前添加 virtual 关键字就提供了这种保证。

    // 子类可以重新定义该方法的行为。
    Virtual void MyChangeableMethod(int someInt);
     
    // 子类必须要重定义该方法。
    Virtual void ThisMethodMustBeDefined(float someFloat) = 0;

虽然允许改变方法的行为方式，但是这并允许我们改变方法的参数类型和数量，以及其返回值类型。父类中的初始版本也并不完全消失。它可以通过作用域操作符进行指定，如下所示：

    void
    ChildClass::DoSomething(void)
    {
    	Printf(“Child class did something\n);
    	ParentClass::DoSomething();
    }

## 静态函数

通常，您需要调用对象的实例才能够使用其方法。有时候，这太麻烦了，难道所有的函数都必须有其“寄主”？我们可以在类的方法前面加上 static 关键字来解除该“魔咒”。静态函数和我们上述的虚函数一样，在调用时需要使用作用域操作符。

    class Myclass
    {
    public:
    				    MyClass(void);
    		    int		DoSomething(void);
    	static 	int		DoSomethingStatic(void);
    };
     
    int
    main(void)
    {
    	MyClass myClassInstance;
     
    	// 调用函数的常用方法
    	MyClassInstance.DoSomething();
     
    	// 下面的函数不需要实例化类。
    	// 和常规的函数调用有略微的不同。
    	MyClass::DoSomethingStatic();
    	return 0;
    }

## 重载：具有相同名称的函数

使用C进行编程时有一个限制，几多英豪曾为此烦恼，任何两个函数都不能够同名，即使它们的参数也不相同。C++ 移除了这个枷锁，因为编译器能够根据参数的数量和类型的不同辨别出它们的区别。如下：

    int MyFunction(int oneWay);
    int MyFunction(char *anotherWay);
    int MyFunction(float aThirdWay);

但是，下述的例子则不行：

    int SomeMethod(const char *oneConstString);
    int SomeMethod(const char *anotherString, const char *optionalString = NULL);

为何不行呢？如果您忽略 optionalString 参数，编译器将无法理解您的意图。

## 习题

阅读 BeBook 中有关 BApplication，Bwindow 和 Bview 的部分。您无需理解所有的东西，但是请尽量对她们有感觉，在下一节中，我们将要为 Haiku 编织真正的花圈了。
