# 1. 对vector.reserve(n)，但vector.capacity()不等于n
## 问题：
对类`Frame`中的`mvKeys`进行预留空间
```cpp
class Frame
{
public:
    Frame();
    
    vector<cv::KeyPoint> mvKeys;
}
        
Frame()
{
    mvKeys.reserve(n);
}
```
利用构造函数构造`Frame`的对象`currentFrame`
```cpp
Frame currentFrame = Frame()
```
此时`currentFrame.mvKeys.capacity()`的结果为0，但是在`Frame`类中`this->mvKeys.capacity()`
的结果为`n`。
## 分析：
这是因为构造函数将构造出来的类 `Frame()` 拷贝(应该是深拷贝)到对象 `currentFrame`的时候，对于
`vector`类所留的内存空间仅为`vector.size`而不是预留空间大小。也就是说 `vector` 的 `reserve`
不会拷贝而生效。这个可能是为了尽可能节约内存栈的空间。