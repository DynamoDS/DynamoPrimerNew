# Verwenden von COM-Typen (Interop-Typen) in Dynamo-Paketen

## Einige allgemeine Informationen zu Interop-Typen
Was sind COM-Typen? - [https://learn.microsoft.com/de-de/windows/win32/com/the-component-object-model](https://learn.microsoft.com/de-de/windows/win32/com/the-component-object-model )

Die Standardmethode zur Verwendung von COM-Typen in C# besteht darin, die primären Interop-Assemblys (im Wesentlichen eine große Sammlung von APIs) mit dem Paket zu referenzieren und zu liefern. 

Eine Alternative dazu besteht darin, die PIA (primäre Interop-Assemblys) in die verwaltete Assembly einzubetten. Dies umfasst im Grunde nur die Typen und Elemente, die tatsächlich von einer verwalteten Assembly verwendet werden. Dieser Ansatz weist jedoch noch einige andere Probleme auf, wie z. B. die Typäquivalenz.

Dieser Post beschreibt das Problem ziemlich gut: 
* [https://learn.microsoft.com/de-de/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/de-de/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## So verwaltet Dynamo die Typäquivalenz
Dynamo delegiert Typäquivalenzen an die .NET-Runtime (dotnet). Ein Beispiel hierfür wäre, dass zwei Typen mit demselben Namen und Namensbereich, die aus unterschiedlichen Assemblys stammen, nicht als äquivalent angesehen werden und Dynamo beim Laden der in Konflikt stehenden Assemblys einen Fehler ausgibt. Bei Interop-Typen überprüft Dynamo mithilfe der [IsEquivalentTo-API](https://learn.microsoft.com/de-de/dotnet/api/system.type.isequivalentto), ob die Interop-Typen äquivalent sind.

## So vermeiden Sie Konflikte zwischen eingebetteten Interop-Typen
Einige Pakete wurden bereits mit eingebetteten Interop-Typen erstellt (z. B. CivilConnection). Das Laden von zwei Paketen mit eingebetteten Interop-Assemblys, die nicht als äquivalent angesehen werden (unterschiedliche Versionen, wie [hier](https://learn.microsoft.com/de-de/dotnet/framework/interop/type-equivalence-and-embedded-interop-types) definiert), führt zu einem `package load error`. Aus diesem Grund empfehlen wir Paketautoren, die dynamische Bindung (auch späte Bindung genannt) für Interop-Typen zu verwenden (die Lösung wird auch [hier](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies) beschrieben).

Sie können sich dieses Beispiel ansehen:
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