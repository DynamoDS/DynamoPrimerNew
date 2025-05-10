# 在 Dynamo 套件中使用 COM (互通性) 類型

## 一些有關互通性類型的一般資訊

什麼是 COM 類型？- [https://learn.microsoft.com/zh-tw/windows/win32/com/the-component-object-model](https://learn.microsoft.com/zh-tw/windows/win32/com/the-component-object-model)

在 C# 中使用 COM 類型的標準方法，是使用套件來參考和遞送主要的互通性組合 (基本上是 API 的一個大型集合)。

另一種方法是將 PIA (主要互通性組合) 嵌入受管的組合中。這基本上只是納入受管組合實際使用的類型和成員。但是，此方法還存在一些其他問題，例如類型對等項。

這篇文章仔細描述了這個問題：

* [https://learn.microsoft.com/zh-tw/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/zh-tw/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Dynamo 如何管理類型對等項

Dynamo 將類型對等項委派給 .NET (dotnet) 執行階段。例如，來自不同組合且具有相同名稱和名稱空間的 2 個類型將不會視為對等項，Dynamo 在載入衝突的組合時將產生錯誤。至於互通性類型，Dynamo 將使用 [IsEquivalentTo API](https://learn.microsoft.com/zh-tw/dotnet/api/system.type.isequivalentto?view=net-9.0) 檢查互通性類型是否為對等項

## 如何避免嵌入式互通性類型之間發生衝突

一些套件已使用嵌入式互通性類型建立 (例如 CivilConnection)。載入 2 個包含不是對等項的嵌入式互通性組合的套件 ([這裡](https://learn.microsoft.com/zh-tw/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)定義不同版本) 將導致 `package load error`。因此，我們建議套件作者對互通性類型使用動態繫結 (也稱為後期繫結) ([這裡](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)也介紹了解決方法)。

您可使用此範例：

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
