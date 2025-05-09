# Dynamo パッケージで COM (相互運用)タイプを使用する

## 相互運用機能タイプに関する一般的な情報

COM タイプとは - [https://learn.microsoft.com/ja-jp/windows/win32/com/the-component-object-model](https://learn.microsoft.com/ja-jp/windows/win32/com/the-component-object-model)

C# で COM タイプを使用する標準的な方法は、主要な相互運用機能アセンブリ(基本的には API の大きなコレクション)を参照してパッケージに配布することです。

これに代わる方法は、管理対象アセンブリに PIA (プライマリ相互運用機能アセンブリ)を埋め込むことです。これには基本的に、管理対象アセンブリで実際に使用されるタイプとメンバーのみが含まれます。ただし、このアプローチには、タイプの等価性など、他にもいくつかの問題があります。

こちらの投稿では、この問題をかなり詳しく説明しています。

* [https://learn.microsoft.com/ja-jp/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/ja-jp/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Dynamo でタイプの等価性を管理する方法

Dynamo は、同等のタイプを .NET (dotnet)ランタイムに委任します。たとえば、名前と名前空間が同じで、異なるアセンブリからの 2 つのタイプは同等とは見なされず、競合するアセンブリを読み込むときに Dynamo でエラーが発生します。相互運用タイプについて、Dynamo は [IsEquivalentTo API](https://learn.microsoft.com/ja-jp/dotnet/api/system.type.isequivalentto?view=net-9.0) を使用して、相互運用タイプが同等かどうかを確認します。

## 埋め込まれた相互運用タイプ間の競合を回避する方法

一部のパッケージは、埋め込まれた相互運用タイプ(例: CivilConnection)を使用して既に作成されています。同等とは見なされない相互運用機能アセンブリが埋め込まれた 2 つのパッケージ([こちら](https://learn.microsoft.com/ja-jp/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)で定義されている異なるバージョン)をロードすると、`package load error` が発生します。このため、パッケージ作成者は、相互運用タイプに動的バインディング(遅延バインディング)を使用することをお勧めします(ソリューションについては[こちら](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)でも説明されています)。

この例を参考にしてください。

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
