# Uso de tipos COM (interoperabilidad) en paquetes de Dynamo

## Información general sobre los tipos de interoperabilidad
¿Qué son los tipos COM? - [https://learn.microsoft.com/es-es/windows/win32/com/the-component-object-model](https://learn.microsoft.com/es-es/windows/win32/com/the-component-object-model )

La forma estándar de usar los tipos COM en C# es hacer referencia a los montajes de interoperabilidad principales (básicamente una gran colección de API) y enviarlos con el paquete. 

Una alternativa es incrustar los PIA (ensamblajes de interoperabilidad principales) en el ensamblaje administrado. Básicamente, esto incluye solo los tipos y miembros que utiliza realmente un ensamblaje administrado. Sin embargo, este enfoque tiene otros problemas, como la equivalencia de tipos.

En esta publicación se describe el problema bastante bien: 
* [https://learn.microsoft.com/es-es/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/es-es/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Cómo gestiona Dynamo la equivalencia de tipos
Dynamo delega la equivalencia de tipos a .NET (dotnet) runtime. Un ejemplo de esto sería que dos tipos con el mismo nombre y espacio de nombres, procedentes de diferentes ensamblajes, no se consideren equivalentes, y Dynamo devuelva un error al cargar los ensamblajes en conflicto. En cuanto a los tipos de interoperabilidad, Dynamo comprueba si los tipos de interoperabilidad son equivalentes mediante la API [IsEquivalentTo](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto).

## Cómo evitar conflictos entre tipos de interoperabilidad incrustados
Algunos paquetes ya se han creado con tipos de interoperabilidad incrustados (por ejemplo, CivilConnection). Si se cargan dos paquetes con ensamblajes de interoperabilidad incrustados que no se consideran equivalentes (versiones diferentes según lo definido [aquí](https://learn.microsoft.com/es-es/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)), se producirá un error `package load error`. Por este motivo, se recomienda que los autores de paquetes usen el enlace dinámico (también conocido como enlace tardío) para los tipos de interoperabilidad (la solución también se describe [aquí](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

Puede seguir este ejemplo:
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