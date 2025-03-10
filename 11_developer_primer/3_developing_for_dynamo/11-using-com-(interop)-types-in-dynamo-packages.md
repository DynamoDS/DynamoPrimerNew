# Usar tipos COM (interoperabilidade) em pacotes do Dynamo

## Algumas informações gerais sobre tipos de interoperabilidade
O que são tipos COM? – [https://learn.microsoft.com/pt-br/windows/win32/com/the-component-object-model](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model )

A maneira padrão de usar tipos COM em C# é fazer referência e enviar as montagens de interoperabilidade principais (basicamente uma grande coleção de APIs) com o pacote. 

Uma alternativa a isso é incorporar as PIA (montagens de interoperabilidade principais) na montagem gerenciada. Isso basicamente inclui apenas os tipos e membros que são realmente usados por uma montagem gerenciada. No entanto, essa abordagem apresenta alguns outros problemas, como a equivalência de tipos.

Esta publicação descreve muito bem o problema: 
* [https://learn.microsoft.com/pt-br/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Como o Dynamo gerencia a equivalência de tipos
O Dynamo delega a equivalência de tipos ao tempo de execução do .NET (dotnet). Um exemplo disso seria que dois tipos com o mesmo nome e namespace, provenientes de montagens diferentes não são considerados equivalentes, e o Dynamo mostrará um erro ao carregar as montagens em conflito. Quanto aos tipos de interoperabilidade, o Dynamo verificará se os tipos de interoperabilidade são equivalentes usando a [API IsEquivalentTo](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto)

## Como evitar conflitos entre tipos de interoperabilidade integrados
Alguns pacotes já foram criados com tipos de interoperabilidade integrados (ex.: CivilConnection). Carregar dois pacotes com montagens de interoperabilidade integradas que não são consideradas equivalentes (versões diferentes, conforme definido [aqui](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)) causará um `package load error`. Devido a isso, sugerimos que os autores dos pacotes usem a vinculação dinâmica (também conhecida como vinculação tardia) para tipos de interoperabilidade (a solução também é descrita [aqui](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

Você pode seguir este exemplo:
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