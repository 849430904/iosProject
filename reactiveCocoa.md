* 代码常用调用方式

````
@implementation ViewController

//不写的返回值为id
- foo{
    return nil;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    //方法调用
    [self foo];
    [ViewController instance];
    
    //2,block调用
    void (^ foo1)()=^{
    
    };
    foo1();
    
    //3, target 方式调用 略
    //UIButton *btn = [UIButton new];
    //btn addTarget:<#(nullable id)#> action:<#(nonnull SEL)#> forControlEvents:<#(UIControlEvents)#>

    //4，delegate方式调用
    UITableView *tableView = [UITableView new];
    tableView.delegate = self;
    tableView.dataSource = self;
    
    NSBlockOperation *block = [NSBlockOperation blockOperationWithBlock:^{
        //
    }];
    [block start];
    [[NSOperationQueue mainQueue]addOperation:block];
}

+ instance{
    return nil;
}
//以下是tableView的代理方法，略
@end
````

* 信号量初体验

````

#import "ViewController.h"
@import ReactiveCocoa;//引入

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    //每次执行viewDidAppear的时候，执行以下代码
    RACSignal* viewDidAppearSignal = [self rac_signalForSelector:@selector(viewDidAppear:)];
    [viewDidAppearSignal subscribeNext:^(id x) {
        NSLog(@"%s",__func__);
    }];
    [viewDidAppearSignal subscribeError:^(NSError *error) {
        //
    }];
    [viewDidAppearSignal subscribeCompleted:^{
        //
    }];
}


- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [self setNeedsStatusBarAppearanceUpdate];
    NSLog(@"%s",__func__);
}
@end
````

button监听:

````
    UIButton *btn = [UIButton new];
    RACCommand *command = [[RACCommand alloc]initWithEnabled:nil signalBlock:^RACSignal *(id input) {
        return  [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
            NSLog("click...");
            [subscriber sendNext:[[NSDate date] description]];
            return [RACDisposable disposableWithBlock:^{
                //
            }];
        }];
    }];
    [btn setRac_command:command];

````

* 监听一个对象的属性的变化 [参考](WTKMVVMRAC-master-master/WTKWineMVVM/WTKWineMVVM/Class/Based/WTKTabBarController.m)

````
//    [[WTKUser currentUser] addObserver:self forKeyPath:@"bageValue" options:NSKeyValueObservingOptionNew context:nil];
//也可以用上面的通知，控制器释放的时候要移除通知
- (void)observerBadgeValue
{
    @weakify(self);
    [RACObserve([WTKUser currentUser], bageValue) subscribeNext:^(id x) {
        @strongify(self);
        UIViewController *vc = self.viewControllers[3];
        NSInteger num = [x integerValue];
        
        dispatch_async(dispatch_get_main_queue(), ^{
            if (num > 0)
            {
                [vc.tabBarItem setBadgeValue:[NSString stringWithFormat:@"%ld",num]];
            }
            else
            {
                [vc.tabBarItem setBadgeValue:nil];
            }
        });
        
    }];
}

````