# Używanie typów COM (międzyoperacyjnych) w pakietach dodatku Dynamo

## Niektóre ogólne informacje o typach międzyoperacyjnych
Czym są typy COM? — [https://learn.microsoft.com/pl-pl/windows/win32/com/the-component-object-model](https://learn.microsoft.com/pl-pl/windows/win32/com/the-component-object-model )

Standardowym sposobem używania typów COM w języku C# jest odwoływanie się do podstawowych zespołów międzyoperacyjnych (w zasadzie dużej kolekcji interfejsów API) i dostarczanie ich z pakietem. 

Alternatywą do tego jest osadzenie PIA (podstawowych zespołów międzyoperacyjnych) w zarządzanym zespole. Zasadniczo obejmuje to tylko te typy i składniki, które są rzeczywiście używane przez zarządzany zespół. Jednak to podejście wiąże się z pewnymi innymi problemami, takimi jak równoważność typów.

Ten wpis dość dobrze opisuje ten problem: 
* [https://learn.microsoft.com/pl-pl/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/pl-pl/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Jak dodatek Dynamo zarządza równoważnością typów
Dodatek Dynamo deleguje równoważność typów do środowiska wykonawczego platformy .NET. Przykładem tego jest to, że 2 typy o tej samej nazwie i przestrzeni nazw pochodzące z różnych zespołów nie będą uznawane za równoważne, a dodatek Dynamo zwróci błąd podczas wczytywania zespołów powodujących konflikt. Jeśli chodzi o typy międzyoperacyjne, dodatek Dynamo sprawdzi, czy te typy międzyoperacyjne są równoważne, przy użyciu [interfejsu API IsEquivalentTo](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto)

## Jak uniknąć konfliktów między osadzonymi typami międzyoperacyjnymi
Niektóre pakiety są już utworzone z osadzonymi typami międzyoperacyjnymi (np. CivilConnection). Wczytanie 2 pakietów z osadzonymi zespołami międzyoperacyjnymi, które nie są uznawane za równoważne (różne wersje, jak zdefiniowano [tutaj](https://learn.microsoft.com/pl-pl/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)), spowoduje błąd `package load error`. Z tego powodu sugerujemy, aby autorzy pakietów używali dla typów międzyoperacyjnych powiązania dynamicznego (znanego również jako powiązanie późne) — rozwiązanie opisano również [tutaj](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies).

Można posłużyć się następującym przykładem:
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