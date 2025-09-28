```cpp
// 1. 定义回调函数类型（函数指针类型）
typedef void (*CallbackFunc)(int);

// 2. 实际的回调函数
void onTaskCompleted(int result) {
    printf("哇！任务完成了！结果是: %d\n", result);
}

// 3. 接收回调函数的函数
void doSomethingAsync(CallbackFunc callback) {
    printf("开始执行任务...\n");
    // 假设这里是一些耗时操作
    int result = 42;
    printf("任务执行完毕，准备调用回调函数...\n");
    // 操作完成，调用回调函数
    callback(result);
}

// 4. 主函数
int main() {
    // 把回调函数传递过去
    doSomethingAsync(onTaskCompleted);
    return 0;
}
```

其实就是传函数进去，**函数变量**

### 1. C语言中的函数指针

这是最基础的方式，就像我们前面看到的：

```c
typedef void (*Callback)(int);

void someFunction(Callback cb) {
    // ...
    cb(42);
}
```

### 2. C++中的函数对象（Functor）

```c++
// 函数对象类
class PrintCallback {
public:
	// 重载()运算符，使该类的对象可以像函数一样被调用
    void operator()(int value) {
        std::cout << "值是: " << value << std::endl;
    }
};

// 接收函数对象的函数
template<typename Func>
void doSomething(Func callback) {
    callback(100);
}

int main() {
    PrintCallback printer;
    doSomething(printer);  // 输出: 值是: 100
    return 0;
}
```

### 3. C++11中的 std::function 和 lambda 表达式

这是最现代的方式，也最灵活：

```c++
// 使用std::function
// 尖括号<>里的内容指定了函数的"签名"：
// - void：表示这个函数没有返回值
// - (int)：表示这个函数接收一个int类型的参数
// 整体意思是：callback是一个"接收int参数且无返回值"的函数
void doTask(std::function<void(int)> callback) {
    callback(200);
}

int main() {
    // 使用lambda表达式
    // lambda表达式是一种匿名函数，格式为：[捕获列表](参数列表){函数体}
    doTask([](int value) {
        std::cout << "Lambda被调用，值是: " << value << std::endl;
    });
    
    // 带捕获的lambda
    int factor = 10;
    doTask([factor](int value) {
        std::cout << "结果是: " << value * factor << std::endl;
    });
    
    return 0;
}
```









功能：显示经过SLAM算法处理后的配准点云

# ROS2连接：订阅 /cloud_registered 话题

# 数据来源：LaserMappingNode::publish_frame_world()函数发布的世界系点云

# 数据类型：sensor_msgs::msg::PointCloud2

# 说明：这是FAST-LIO2算法的主要输出，显示当前帧LiDAR数据在世界坐标系下的位置

- Alpha: 1 # 点云不透明度

Autocompute Intensity Bounds: true # 自动计算强度值范围