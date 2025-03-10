# Dynamo 패키지에서 COM(interop) 유형 사용

## interop 유형에 대한 몇 가지 일반 정보
COM 유형이란? - [https://learn.microsoft.com/ko-kr/windows/win32/com/the-component-object-model](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model )

C#에서 COM 유형을 사용하는 표준 방법은 기본 interop 어셈블리(기본적으로 많은 API를 모아 놓은 것)를 참조하고 패키지와 함께 배포하는 것입니다. 

이 방법의 대안은 관리되는 어셈블리에 PIA(Primary Interop Assembly)를 포함시는 것입니다. 이 대안 방법에서는 기본적으로 관리되는 어셈블리에서 실제로 사용되는 유형과 멤버만 포함됩니다. 그러나 이 접근 방식에는 유형 등가성과 같은 몇 가지 다른 문제가 있습니다.

아래 게시글에 해당 문제가 잘 설명되어 있습니다. 
* [https://learn.microsoft.com/ko-kr/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Dynamo에서 유형 등가성을 관리하는 방법
Dynamo는 유형 등가성을 .NET(dotnet) 런타임에 위임합니다. 예를 들어, 동일한 이름과 네임스페이스를 가진 두 개의 유형이 서로 다른 어셈블리에서 오는 경우, 이들은 동일한 것으로 간주되지 않으며 충돌하는 어셈블리를 로드할 때 Dynamo에서 오류가 발생합니다. interop 유형의 경우 Dynamo는 [IsEquivalentTo API](https://learn.microsoft.com/en-us/dotnet/api/system.type.isequivalentto)를 사용하여 interop 유형이 동일한지 확인합니다.

## 포함된 interop 유형 간의 충돌을 방지하는 방법
일부 패키지는 이미 포함된 interop 유형(예: CivilConnection)으로 생성되었습니다. 동일한 것으로 간주되지 않는 interop 어셈블리가 포함된 2개의 패키지([여기](https://learn.microsoft.com/en-us/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)에 정의된 대로 다른 버전)를 로드하면 `package load error`가 발생합니다. 이로 인해 패키지 작성자는 interop 유형에 대해 동적 바인딩(지연 바인딩이라고도 함)을 사용하는 것이 좋습니다(해결 방법도 [여기](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)에 설명되어 있음).

다음 예를 참고할 수 있습니다.
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