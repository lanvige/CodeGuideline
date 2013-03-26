#ObjC #Code Guideline
---

###命名风格

- 文件名首字母大写，以后每个单词首字母大写
- 类的定义和文件名相同
- 属性和方法则是首字母小写，以后每个单词首字母大写
- Controls命名用全写，如下
	- userAvatarImageView 
	- deleteButton
- Controls对应的IBAction命名如下
	- deleteButtonAction


###逻辑代码风格

- 方法定义，大括号位于方法下面，每个括号都占单独一行

	`- (void)xxxx	
	{	
	}
	`

- 逻辑的写法，大括号则位于同行

	`if (xxx == yyy)  {

	}
	`

- (void)setSource:(HNObject *)source_ 
所有赋值方法后的参数名都是name_



- (id)init
放对象初始化所需要的数据
 
- (void)loadView
视图创建的东西，像UITableView的创建，属性设置，addSubView

- (void)viewDidLoad



\#pragma mark -
\#pragma mark Interface Orientation

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    return (interfaceOrientation != UIInterfaceOrientationPortraitUpsideDown);
}

###CocoaTouch

- View初始化

	@property (nonatomic, strong) SSLabel *upgradeLabel;
	
	\- (SSLabel *)upgradeLabel {
	     if (!_upgradeLabel) {
	          _upgradeLabel = [[SSLabel alloc] initWithFrame:CGRectMake(0.0f, 0.0f, self.view.frame.size.width, 60.0f)];
	          _upgradeLabel.font = [UIFont cheddarInterfaceFontOfSize:14.0f];
	          _upgradeLabel.autoresizingMask = UIViewAutoresizingFlexibleWidth;
	          _upgradeLabel.userInteractionEnabled = YES;
	     }
	     return _upgradeLabel;
	}

# 注释

- \#pragma mark - NSObject
	- init
	- dealloc
- \#LifeCycle
	- ViewDidLoad
	- ViewWillApear
- 每个Delegate都拥有一个注释区
	- \#pragma mark - UITableViewDelegate Methods
	- \#pragma mark - UITableViewDataSource Methods
- \#pragma mark - Actions 
- \#pragma mark - Gestures
- \#pragma mark - Memory Management Methods
	- ViewDidUnload
	- (void)didReceiveMemoryWarning 

###性能
- 头文件中的引用使用@class，而非#import，import放在.m文件中
	- @class性能会高一点，因为编译器不需要处理整个ClassName.h文件，只需要知道ClassName是一个类名字。
	- 如果需要引用该类中的方法，则@class是不行的，因为编译器需要更多的消息。要知道方法有多少参数，什么类型，方法的返回类型。
- 线程写法
	- // Defer some stuff to make launching faster
dispatch_async(dispatch_get_main_queue(), ^{
});

###ARC

###XCode 4.4 新语法

#####NSNumber
- // 单个字符
	- NSNumber *theLetterZ = @'Z';   // 相当于 [NSNumber numberWithChar:'Z']

- // 整形
	- NSNumber *fortyTwo = @42;      // 相当于 [NSNumber numberWithInt:42]
	- NSNumber *ftUnsigned = @42U;   // 相当于 [NSNumber numberWithUnsignedInt:42U]
	- NSNumber *ftLong = @42L;       // 相当于 [NSNumber numberWithLong:42L]
	- NSNumber *ftLongLong = @42LL;  // 相当于 [NSNumber numberWithLongLong:42LL]

- // 浮点
	- NSNumber *piFloat = @3.141592F;// 相当于 [NSNumber numberWithFloat:3.141592F]
	- NSNumber *piDouble = @3.141592;// 相当于 [NSNumber numberWithDouble:3.141592]

- // 是 / 否
	- NSNumber *yesNumber = @YES;    // 相当于 [NSNumber numberWithBool:YES]
	- NSNumber *noNumber = @NO;      // 相当于 [NSNumber numberWithBool:NO]

######NSArray
- NSArray = @[obj, obj];

######NSDictionary
NSDictionary *options = @{
    @"backup": @YES,
    @"daysToKeepBackup": @7,
    @"flags": @"foo"
};

######嵌套表达式 (Boxed Expressions)

- 最新版本的 Objective-C 还提供了一种新的书写方式:
	- @( expression )

######Property

- 升级到 Xcode 4.4 后，在头文件中创建的 @property 均无需再进行 @synthesize。Xcode 将自动生成。
	- @synthesize object = _object;