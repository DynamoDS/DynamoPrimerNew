# Použití typů COM (interoperability) v balíčcích aplikace Dynamo

## Obecné informace o typech interoperability
Co jsou typy COM? – [https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model )

Standardním způsobem, jak používat typy COM v jazyce C#, je odkazovat a dodávat primární sestavení interoperability (v podstatě velká kolekce rozhraní API) s balíčkem. 

Alternativou k tomu je vložení PIA (primary interop assemblies – primární sestavení interoperability) do spravovaného sestavení. To v podstatě zahrnuje pouze typy a členy, které jsou skutečně používány spravovaným sestavením. Tento přístup má však některé další problémy, jako je ekvivalence typů.

Tento článek tuto problematiku dobře popisuje: 
* [https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Jak Dynamo spravuje ekvivalenci typů
Dynamo deleguje ekvivalenci typů na modul runtime .NET. Například 2 typy se stejným názvem a jmenným prostorem, pocházející z různých sestavení, nebudou považovány za ekvivalentní a Dynamo nahlásí chybu při načítání konfliktních sestavení. Pokud jde o typy interoperability, Dynamo zkontroluje, zda jsou ekvivalentní pomocí rozhraní API [IsEquivalentTo](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto).

## Jak se vyhnout konfliktům mezi vloženými typy interoperability
Některé balíčky již byly vytvořeny s vloženými typy interoperability (např. CivilConnection). Načtení 2 balíčků s vloženými sestaveními interoperability, která nejsou považována za ekvivalentní (různé verze, jak je definováno [zde](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)), způsobí chybovou zprávu `package load error`. Z tohoto důvodu doporučujeme, aby autoři balíčků používali pro typy interoperability dynamickou vazbu (tzv. pozdní vazbu) (řešení je také popsáno [tady](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

Můžete postupovat podle tohoto příkladu:
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