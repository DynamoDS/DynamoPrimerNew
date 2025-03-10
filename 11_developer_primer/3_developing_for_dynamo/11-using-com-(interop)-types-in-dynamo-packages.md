# Utilisation de types COM (interopérabilité) dans les packages Dynamo

## Informations générales sur les types d’interopérabilité
Que sont les types COM ? - [https://learn.microsoft.com/fr-fr/windows/win32/com/the-component-object-model](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model )

La manière standard d’utiliser les types COM en C# consiste à référencer et à expédier les assemblages d’interopérabilité de base (une grande collection d’API) avec votre package. 

Vous pouvez également incorporer les PIA (assemblages d’interopérabilité de base) dans votre assemblage géré. Cela inclut uniquement les types et les membres qui sont réellement utilisés par un assemblage géré. Cependant, cette approche pose d’autres problèmes comme l’équivalence des types.

Cet article décrit assez bien le problème : 
* [https://learn.microsoft.com/fr-fr/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Comment Dynamo gère l’équivalence des types ?
Dynamo délègue l’équivalence des types à l’environnement d’exécution .NET (dotnet). Par exemple, si deux types avec le même nom et le même espace de noms proviennent d’assemblages différents, ils ne seront pas considérés comme équivalents, et Dynamo signalera une erreur lors du chargement des assemblages en conflit. En ce qui concerne les types d’interopérabilité, Dynamo vérifie si les types d’interopérabilité sont équivalents à l’aide de l’[API IsEquivalentTo](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto)

## Comment éviter les conflits entre les types d’interopérabilité intégrés ?
Certains packages ont déjà été créés avec des types d’interopérabilité intégrés (par exemple, CivilConnection). Le chargement de deux packages avec des assemblages d’interopérabilité intégrés qui ne sont pas considérés comme équivalents (versions différentes telles que définies [ici](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)) entraînera une `package load error`. Pour cette raison, nous suggérons aux créateurs de packages d’utiliser la liaison dynamique (aussi appelée la liaison tardive) pour les types d’interopérabilité (la solution est également décrite [ici](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

Vous pouvez suivre cet exemple :
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