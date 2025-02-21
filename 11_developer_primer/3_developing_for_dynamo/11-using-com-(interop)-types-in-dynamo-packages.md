# Using COM (interop) types in Dynamo Packages

## Some general information about interop types
What are COM types? - https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model 

The standard way to use COM types in C# is to reference and ship the primary interop assemblies (Basically a big collection of APIs) with your package. 

An alternative to this is to embed the PIA (Primary interop assemblies) in your managed assembly. This basically only includes the types and members that are actually used by a managed assembly. However this approach has some other issues like type equivalence.

This post describes the issue pretty well: 
* https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types

## How Dynamo manages types equivalence
Dynamo delegates type equivalence to the .NET (dotnet) runtime. 
An example of this would be that 2 types with the same name and namespace, coming from different assemblies will not be considered equivalent, and Dynamo will give an error when loading the conflicting assemblies. 
As for interop types, Dynamo will check if the interop types are equivalent using the [IsEquivalentTo API](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto)

## How to avoid conflicts between embedded interop types
Some packages have already been created with embedded interop types (ex CivilConnection). 
Loading 2 packages with embedded interop assemblies that are not considered equivalent(different versions as defined [here](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)) will cause a `package load error`.
Because of this, we suggest that package authors use dynamic binding (aka late binding) for interop types (solution is also described [here](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

You can follow this example:
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