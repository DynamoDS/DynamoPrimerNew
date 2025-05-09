# Utilizzo di tipi COM (interoperabilità) nei pacchetti di Dynamo

## Alcune informazioni generali sui tipi di interoperabilità

Cosa sono i tipi COM? - [https://learn.microsoft.com/it-it/windows/win32/com/the-component-object-model](https://learn.microsoft.com/it-it/windows/win32/com/the-component-object-model)

Il modo standard per utilizzare i tipi COM in C# consiste nel fare riferimento agli assiemi di interoperabilità primari (in pratica una grande raccolta di API) con il pacchetto.

Un'alternativa a questa operazione consiste nell'incorporare i PIA (assiemi di interoperabilità primari) nell'assieme gestito. Ciò include sostanzialmente solo i tipi e i membri che sono effettivamente utilizzati da un assieme gestito. Tuttavia, questo approccio presenta altri problemi, come l'equivalenza dei tipi.

In questo post è descritto abbastanza bene il problema:

* [https://learn.microsoft.com/it-it/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/it-it/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## In che modo Dynamo gestisce l'equivalenza dei tipi

Dynamo delega l'equivalenza dei tipi al runtime .NET (dotnet). Ad esempio, 2 tipi con lo stesso nome e lo stesso spazio dei nomi, provenienti da assiemi diversi, non saranno considerati equivalenti e in Dynamo verrà visualizzato un errore durante il caricamento degli assiemi in conflitto. Per quanto riguarda i tipi di interoperabilità, Dynamo consentirà di verificare se i tipi di interoperabilità sono equivalenti utilizzando l'[API IsEquivalentTo](https://learn.microsoft.com/it-it/dotnet/api/system.type.isequivalentto).

## Come evitare conflitti tra tipi di interoperabilità incorporati

Alcuni pacchetti sono già stati creati con tipi di interoperabilità incorporati (ad esempio CivilConnection). Il caricamento di 2 pacchetti con assiemi di interoperabilità incorporati che non sono considerati equivalenti (versioni diverse come definito [qui](https://learn.microsoft.com/it-it/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)) causerà `package load error`. Per questo motivo, è consigliabile che gli autori dei pacchetti utilizzino l'associazione dinamica (nota anche come associazione tardiva) per i tipi di interoperabilità (la soluzione è descritta anche [qui](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

È possibile seguire questo esempio:

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
