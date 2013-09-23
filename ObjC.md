#ObjC #Code Guideline
---

###命名风格

- 文件名首字母大写，以后每个单词首字母大写
- 类的定义和文件名相同
- 属性和方法则是首字母小写，以后每个单词首字母大写
- 常量的定义用k开头，后面单词首字母大写 `kAFNetworkingDownload`
- Controls命名用全写，如下
	- userAvatarImageView 
	- deleteButton
- Controls对应的IBAction命名如下
	- deleteButtonAction


###逻辑代码风格

- 方法定义，大括号位于方法下面，每个括号都占单独一行
``` ObjC
- (void)init	
{	
}
```

- 逻辑的写法，大括号则位于同行
``` ObjC
if (xxx == yyy)  {
} else {
}
```

- Switch

	``` ObjC
	switch (state) {
		case AFOperationReadyState:
			break;
		default:
			break;
	}
	// 若是case里有逻辑语句
	switch (state) {
		case AFOperationReadyState: {
			// logic statement
			break;
		}
		default:
			break;
	}
	```

- 所有赋值方法后的参数名都是name_
``` ObjC
- (void)setSource:(HNObject *)source_ 
{
}
```

- init函数中放对象初始化所需要的数据
``` ObjC
- (id)initWithData:(NSData)data_
{
}
```

- 视图创建的东西，像UITableView的创建，属性设置，addSubView 
``` ObjC
- (void)loadView
```

- viewDidLoad中用来？
``` ObjC
- (void)viewDidLoad
```


###CocoaTouch

- View初始化

	``` ObjC
	@property (nonatomic, strong) SSLabel *upgradeLabel;  
	
	- (SSLabel *)upgradeLabel {
    	if (!_upgradeLabel) {
        	_upgradeLabel = [[SSLabel alloc] initWithFrame:CGRectMake(0.0f, 0.0f, self.view.frame.size.width, 60.0f)];
         	_upgradeLabel.font = [UIFont cheddarInterfaceFontOfSize:14.0f];
         	_upgradeLabel.autoresizingMask = UIViewAutoresizingFlexibleWidth;
         	_upgradeLabel.userInteractionEnabled = YES;
		}
    	return _upgradeLabel;
	}
	```

# 注释

``` ObjC
#pragma mark
#pragma mark - NSObject
	- init
	- dealloc

#pragma mark
#pragma mark - LifeCycle
	- (void)viewDidLoad
	- ViewWillApear

//每个Delegate都拥有一个注释区
#pragma mark
#pragma mark - UITableViewDelegate Methods

#pragma mark
#pragma mark - UITableViewDataSource Methods

#pragma mark
#pragma mark - Actions 

#pragma mark
#pragma mark - Gestures

#pragma mark
#pragma mark - Memory Management Methods
	- (void)viewDidUnload
	- (void)didReceiveMemoryWarning 

#pragma mark -
#pragma mark Interface Orientation

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    return (interfaceOrientation != UIInterfaceOrientationPortraitUpsideDown);
}
```

###性能

- 头文件中的引用使用@class，而非#import，import放在.m文件中
	- @class性能会高一点，因为编译器不需要处理整个ClassName.h文件，只需要知道ClassName是一个类名字。
	- 如果需要引用该类中的方法，则@class是不行的，因为编译器需要更多的消息。要知道方法有多少参数，什么类型，方法的返回类型。
- 线程写法

	``` ObjC
	// Defer some stuff to make launching faster
	dispatch_async(dispatch_get_main_queue(), ^{
	});
	```
- 常量定义
	- #define 宏
	
	```
	#define kDefaultDomainURL @"http.service.urlprefix.default"
	// 这种方式定义后，可在其它地方直接使用，方便简单，其就是在编译时期为作编译参数传入，编译时不会对其进行类型检测，也无法对常量进行指针比较操作。适合于？
	```
	- extern
	
	```
	// .h define
	extern NSString *const kFontDefaultsKey;
	// .m implenment
	NSString *const kFontDefaultsKey = @"FontDefaults";
	..
	// 这是推荐的定义全局字符常量的方式
	// 修饰符extern用在变量或者函数的声明前，用来说明“此变量/函数是在别处定义的，要在此处引用”。
	// extern 使得该常量可以在同一个工程的其它源文件中被访问(只需用extern进行声明即可)。
	```
	- static

	```
	static NSString *const cellIdentifier = @"cellIdentifier";
	// static 可以用来修饰局部变量，全局变量
	```
	- const

	```
	NSString *const kFontDefaultsKey = @"FontDefaults";
	// const数据成员只在某个对象生存期内是常量，而对于整个类而言却是可变的。因为类可以创建多个对象，不同的对象其const数据成员的值可以不同。所以不能在类声明中初始化const数据成员，因为类的对象未被创建时，编译器不知道const 数据成员的值是什么。
	// const数据成员的初始化只能在类的构造函数的初始化表中进行。要想建立在整个类中都恒定的常量，应该用类中的枚举常量来实现，或者static const。
	```

###ARC

###XCode 4.4 新语法

#####NSNumber
```
// 单个字符
NSNumber *theLetterZ = @'Z';   // 相当于 [NSNumber numberWithChar:'Z']

// 整形
NSNumber *fortyTwo = @42;      // 相当于 [NSNumber numberWithInt:42]
NSNumber *ftUnsigned = @42U;   // 相当于 [NSNumber numberWithUnsignedInt:42U]
NSNumber *ftLong = @42L;       // 相当于 [NSNumber numberWithLong:42L]
NSNumber *ftLongLong = @42LL;  // 相当于 [NSNumber numberWithLongLong:42LL]

// 浮点
NSNumber *piFloat = @3.141592F;   // 相当于 [NSNumber numberWithFloat:3.141592F]
NSNumber *piDouble = @3.141592;   // 相当于 [NSNumber numberWithDouble:3.141592]

// BOOL
NSNumber *yesNumber = @YES;    // 相当于 [NSNumber numberWithBool:YES]
NSNumber *noNumber = @NO;      // 相当于 [NSNumber numberWithBool:NO]
```

######NSArray

``` ObjC
// Old style
NSArray *items = [NSArray arrayWithObjects:@"item1",
                  [NSNumber numberWithBool:YES],
                  [NSNumber numberWithInt:12], nil];
// New style
NSArray *items = @[ @"item1", @YES, @12 ];
```

######NSDictionary

``` ObjC
// Old style
NSDictionary *options = [NSDictionary dictionaryWithObjectsAndKeys:
                         [NSNumber numberWithBool:YES], @"backup",
                         [NSNumber numberWithInt:7],    @"daysToKeepBackup",
                         @"foo",                        @"flags", nil];
// New style
NSDictionary *options = @{
    @"backup": @YES,
    @"daysToKeepBackup": @7,
    @"flags": @"foo"
};
```

######NS_ENUM

[Objective-C Enumeration, NS_ENUM & NS_OPTIONS](http://stackoverflow.com/questions/14080750/objective-c-enumeration-ns-enum-ns-options)
```ObjC
typedef NS_ENUM(NSInteger, MMType)
{
    MMTypeA,
    MMTypeB
};
```

######嵌套表达式 (Boxed Expressions)

- 最新版本的 Objective-C 还提供了一种新的书写方式

	``` ObjC
	@( expression )
	```

######Property

- 升级到 Xcode 4.4 后，在头文件中创建的 @property 均无需再进行 @synthesize。Xcode 将自动生成

	``` ObjC
	@synthesize object = _object;
	```


####REF::
https://github.com/github/objective-c-conventions
