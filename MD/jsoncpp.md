# JSON
在创建json文件时，应当考虑将json中所有的键值对视作一个数组，由根数组进行保管

示例
```C++
void writeJson()
{
    // 将最外层的数组看做一个Value
    // 最外层的Value对象创建
    Value root;
    // Value有一个参数为int 行的构造函数
    root.append(12);	// 参数进行隐式类型转换
    root.append(12.34);
    root.append(true);
    root.append("tom");
    
    // 创建并初始化一个子数组
    Value subArray;
    subArray.append("jack");
    subArray.append("ace");
    subArray.append("robin");
    root.append(subArray);
    
    // 创建并初始化子对象
    Value subObj;
    subObj["sex"] = "woman";  // 添加键值对
    subObj["girlfriend"] = "lucy";
    root.append(subObj);
    
    // 序列化
#if 1
    // 有格式的字符串
    string str = root.toStyledString();
#else
    FastWriter f;
    string str = f.write(root);
#endif
    // 将序列化的字符串写磁盘文件
    ofstream ofs("test.json");
    ofs << str;
    ofs.close();
}

```
## Value类
支持以下所有类型传入
|类型|说明|
|-|-|
|nullValue|空值|
|intValue|有符号整数|
|uintValue|无符号整数|
|realValue|浮点数|
|stringValue|字符串（UTF-8）|
|booleanValue|布尔值|
|arrayValue|数组（可装不同类型）|
|objectValue|键值对|
###  append函数
Value对象调用的append函数会对参数调用Value类的构造函数
