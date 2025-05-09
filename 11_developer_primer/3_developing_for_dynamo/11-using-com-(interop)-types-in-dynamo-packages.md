# 在 Dynamo 软件包中使用 COM（互操作）类型

## 有关互操作类型的一些常规信息

什么是 COM 类型？- [https://learn.microsoft.com/zh-cn/windows/win32/com/the-component-object-model](https://learn.microsoft.com/zh-cn/windows/win32/com/the-component-object-model)

在 C# 中使用 COM 类型的标准方法是随包一起引用和交付主互操作程序集（基本上是一个大的 API 集合）。

另一种方法是将 PIA（主互操作程序集）嵌入到托管程序集中。这基本上只包括托管程序集实际使用的类型和成员。但是，这种方法还有一些其他问题，例如类型等效性。

这篇文章很好地描述了这个问题：

* [https://learn.microsoft.com/zh-cn/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/zh-cn/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Dynamo 如何管理类型等效

Dynamo 将类型等效委派给 .NET (dotnet) 运行时。例如，来自不同程序集且具有相同名称和名称空间的 2 个类型不被视为等效，Dynamo 会在加载冲突的程序集时显示错误。对于互操作类型，Dynamo 将使用 [IsEquivalentTo API](https://learn.microsoft.com/zh-cn/dotnet/api/system.type.isequivalentto?view=net-9.0) 检查互操作类型是否等效

## 如何避免嵌入式互操作类型之间的冲突

某些软件包已使用嵌入式互操作类型（例如 CivilConnection）创建。加载 2 个软件包（其中包含不被视为等效的嵌入式互操作程序集（[此处](https://learn.microsoft.com/zh-cn/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)定义的不同版本）将导致 `package load error`。因此，我们建议软件包作者对互操作类型使用动态绑定（也称为后期绑定）（解决方案也[此处](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)描述）。

您可以按照以下示例作：

```
public class Application
    {
        string m_sProdID = "SomeNamespace.Application";
        public dynamic m_oApp = null; // Use dynamic so you do not need the PIA types

        public Application()
        {
            this.GetApplication();
        }

        internal void GetApplication()
        {
            try

            {
                m_oApp = System.Runtime.InteropServices.Marshal.GetActiveObject(m_sProdID);
            }
            catch (Exception /*ex*/)
            {}
        }
    }
}
```
