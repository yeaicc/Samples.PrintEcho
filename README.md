### PrintEcho.SDK -- 印刻SDK

这是一个PrintEcho(印刻)打印客户端的SDK。

可以让你开发自定义函数，和安装进印刻打印客户端。

#### 入门

1、创建一个.net8.0类库

2、执行以下命令安装PrintEcho.SDK

```shell
  dotnet add package PrintEcho.SDK
```

或

```shell
  Install-Package PrintEcho.SDK
```

3、创建一个类，用于编写自定义函数，如下所示：

```csharp
using PrintEcho.SDK;
using System;
using Stimulsoft.Report;

namespace Examle
{
    /// <summary>
    /// 包含自定义函数的类
    /// </summary>
    public class MyCustomFunctions
    {
        /// <summary>
        /// 定义您的自定义函数
        /// </summary>
        [CustomFunction("示例")]
        public static string SomeFunction(string name)
        {
            // 自定义函数内可以获取当前报表对象
            var report = PrintContext<StiReport>.Current;
            report.XXXX? // write your code
        }
    }
}
```
4、将类库发布为`release`模式，获得`example.dll`文件

5、将文件读取为二进制后，转为`BASE64`字符串(或将文件发布到网络地址)，调用`API`(`/api/funcs/install`)安装自定义函数

6、安装后，可以调用`API`(`/api/funcs`)获取已安装的自定义函数信息进行验证

#### 标注自定义函数

```csharp
// 引用命名空间
using PrintEcho.SDK;

/// <summary>
/// 按名称获取一段文本
/// </summary>
[CustomFunction("函数分类",  // <-- 标注属性
    Description = "按名称获取一段文本",
    ReturnDescription = "返回一段XX文本",
    ArgumentNames = ["name"], ArgumentDescriptions = ["名称"])
public static string GetText(string name)
{
    return $"Hi {name}, this message is generated by your custom function '{nameof(GetText)}'..";
}
```

#### 自定义函数内获取当前正在渲染的报表对象

```csharp
using PrintEcho.SDK;
using Stimulsoft.Report;

[CustomFunction("获取报表实例示例")]
public static string SomeFunction()
{
    var report = PrintContext<StiReport>.Current;
    report.XXXX? // write your code
}
```