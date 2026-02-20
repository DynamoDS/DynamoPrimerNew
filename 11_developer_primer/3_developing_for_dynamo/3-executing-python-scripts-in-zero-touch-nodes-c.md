# Zero-Touch 노드에서 Python 스크립트 실행하기(C#)

### Zero-Touch 노드에서 Python 스크립트 실행하기(C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Python에서 스크립트를 작성하는 데 익숙하고 표준 Dynamo Python 노드에서 더 많은 기능을 사용하고자 하는 경우 Zero-Touch를 사용하여 자체 노드를 만들 수 있습니다. Python 스크립트를 문자열로 Zero-Touch 노드에 전달하여 스크립트가 실행되고 결과가 반환되는 간단한 예로 시작하겠습니다. 이 사례 연구는 연습과 시작하기 섹션의 예제를 기반으로 합니다. Zero-Touch 노드를 만드는 것이 완전히 처음인 경우 해당 섹션을 참조하십시오.

![Python 스크립트 문자열을 실행할 Zero-Touch 노드](../../.gitbook/assets/python-case-study.png)

> Python 스크립트 문자열을 실행할 Zero-Touch 노드

#### Python 엔진 <a href="#python-engine" id="python-engine"></a>

CPython에서 마이그레이션하는 경우, PythonNet3가 더 매끄러운 사용 경험을 제공하는 기본 엔진으로 설정됩니다. Dynamo 4.0 이상에서 작성된 모든 새 Python 노드는 기본적으로 PythonNet3를 사용합니다.

이 노드는 IronPython 스크립팅 엔진의 인스턴스에 의존합니다. 이를 위해서는 몇 개의 추가 어셈블리를 참조해야 합니다. 아래 단계에 따라 Visual Studio에서 기본 템플릿을 설정합니다.

* 새 Visual Studio 클래스 프로젝트를 생성합니다.
* `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`에 있는 `IronPython.dll`에 참조를 추가합니다.
* `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`에 있는 `Microsoft.Scripting.dll`에 참조를 추가합니다.
* 클래스에 `IronPython.Hosting` 및 `Microsoft.Scripting.Hosting` `using` 문을 포함합니다.
* 추가 노드가 패키지와 함께 Dynamo 라이브러리에 추가되지 않도록 빈 Private 생성자를 추가합니다.
* 단일 문자열을 입력 매개변수로 사용하는 새 메서드를 생성합니다.
* 이 메서드 내에서 새 Python 엔진을 인스턴스화하고 빈 스크립트 범위를 생성합니다. 이 범위를 Python 해석기의 인스턴스 내에 있는 전역 변수라고 생각하면 됩니다.
* 다음으로, 입력 문자열 및 범위를 매개변수로 전달하여 엔진에 대해 `Execute`를 호출합니다
* 마지막으로, 범위에 대해 `GetVariable`을 호출하고 반환하려는 값이 포함된 Python 스크립트의 변수 이름을 전달하여 스크립트 결과를 검색하고 반환합니다. (자세한 내용은 아래 예를 참조하십시오.)

다음 코드는 위에서 언급한 단계의 예를 제공합니다. 솔루션을 빌드하면 프로젝트의 bin 폴더에 새 `.dll`이 생성됩니다. 이제 이 `.dll`을 패키지의 일부로 또는 `File < Import Library...`로 이동하여 Dynamo로 가져올 수 있습니다.

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Python 스크립트가 변수 `output`을 반환하므로 Python 스크립트에 `output` 변수가 있어야 합니다. 이 샘플 스크립트를 사용하여 Dynamo의 노드를 테스트하십시오. Dynamo에서 Python 노드를 사용해 본 적이 있다면 다음이 익숙할 것입니다. 자세한 내용은 [입문서의 Python 섹션](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python)을 참조하십시오.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### 여러 출력 <a href="#multiple-outputs" id="multiple-outputs"></a>

표준 Python 노드의 한 가지 한계는 출력 포트가 하나뿐이므로 여러 객체를 반환하려면 리스트를 구성하고 각 객체를 가져와야 한다는 것입니다. 위의 예를 수정하여 사전을 반환하는 경우 원하는 만큼 출력 포트를 추가할 수 있습니다. 사전에 대한 자세한 내용은’ Zero-Touch로 한 단계 더 나아가기’의 여러 값 반환하기 섹션을 참조하십시오.

![이 노드를 사용하면 직육면체의 체적과 중심을 모두 반환할 수 있습니다.](../../.gitbook/assets/python-multi-case-study.png)

> 이 노드를 사용하면 직육면체의 체적과 중심을 모두 반환할 수 있습니다.

다음 단계를 수행하여 이전 예를 수정해 보겠습니다.

* NuGet 패키지 관리자에서 `DynamoServices.dll`에 참조를 추가합니다.
* 이전 어셈블리에는 `System.Collections.Generic` 및 `Autodesk.DesignScript.Runtime`도 포함되어 있습니다.
* 메서드의 반환 유형을 수정하여 출력이 포함될 사전을 반환합니다.
* 각 출력은 범위에서 개별적으로 검색해야 합니다(더 큰 출력 세트의 경우 간단한 루프를 설정하는 것이 좋음).

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

샘플 Python 스크립트에 추가 출력 변수(`output2`)도 추가했습니다. 이러한 변수는 모든 합법적인 Python 명명 규칙을 사용할 수 있으며, 이 예에서는 명확성을 위해 출력을 정확하게 사용했습니다.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### PythonNet3의 알려진 제한 사항 및 해결 방법<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

다음은 PythonNet3를 사용할 때 알려진 몇 가지 제한 사항과 그에 대한 해결 방법입니다.

* .NET 컬렉션이 Python 리스트로 자동 변환되지 않음
    * ```len()``` 사용, 인덱싱 또는 반복을 수행하기 전에 ```list(...)```를 사용하여 ```.NET``` 배열 또는 컬렉션을 명시적으로 변환해야 합니다.
* 제네릭 .NET 메서드에 명시적인 형식 매개변수가 필요할 수 있음
    * 일부 메서드(예: ```GroupBy```)는 자동 추론에 의존할 경우 실패할 수 있으므로, 제네릭 형식을 수동으로 지정해야 합니다.
* ```dir()``` 또는 자동 완성을 통해 확장 메서드를 탐색할 수 없음
    * 확장 메서드는 명시적으로 호출하면 동작할 수는 있지만, 인트로스펙션 또는 코드 완성에는 표시되지 않습니다.
* DataTable 확장 메서드가 지원되지 않음
    * ```System.Data.DataTableExtensions```를 가져오면 실패하며, 해당 헬퍼 메서드는 직접 사용할 수 없습니다.
* 일부 Dynamo 코어 메서드가 PythonNet3에서 다르게 동작함
    * 컬렉션 처리 방식이 더 엄격해지면서, 일부 함수(예: 리스트 평탄화)가 예상한 대로 동작하지 않을 수 있습니다.
* .NET 형식을 상속한 Python 클래스가 노드 간에 전달되지 않음
    * .NET 형식 또는 인터페이스에서 파생된 클래스는 Python 노드 간에 안전하게 전송될 수 없습니다.
* Python ```set()```이 일부 .NET 객체를 허용하지 않음
    * ```InvalidElementId```와 같은 객체는 필터링하거나 .NET 컬렉션을 사용하여 처리해야 합니다.
* ```print()```를 자주 호출하면 메모리 사용랴이 증가할 수 있음
    * 루프 또는 장시간 실행되는 스크립트에서는 ```print()```를 과도하게 사용하지 않도록 하십시오.
* Dynamo와 Python 간의 사전 상호 운용성이 제한적임
    * Dynamo 사전과 Python 사전은 완전히 호환되지 않으며, 수동으로 변환해야 할 수 있습니다.
* 지정된 객체의 실행 중인 COM 인스턴스를 가져오는 ```Marshal.GetActiveObject()``` 메서드를 더 이상 사용할 수 없음
    * 사용 중인 파일의 경로를 알고 있는 경우 ```BindToMoniker```를 사용하십시오.
    * 클래스 구조 ```Marshal.GetActiveObject()```를 사용하여 C#에서 라이브러리를 작성하십시오.

#### CPython3에서 PythonNet3로 마이그레이션<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo는 CPython 노드를 PythonNet 3로 자동으로 마이그레이션합니다. 진행 과정은 다음과 같습니다.

> 1. 원본 파일의 백업 사본이 자동으로 작성됩니다.
> 2. 모든 CPython 노드(CPython을 사용하는 사용자 지정 노드 포함)가 PythonNet3로 변환됩니다. 
> 3. 얼마나 많은 노드가 마이그레이션되었는지를 알려주는 토스트 알림이 표시됩니다.
> 4. 저장 시, Python 노드가 이제 PythonNet3를 사용하게 된다는 안내 메시지가 표시됩니다. 다시 한 번 말씀드리지만, 이전 버전과의 호환성에 대해 걱정하지 않아도 됩니다. 여러 버전을 병행 사용하는 환경(예: Revit 또는 Civil 3D 2025/2026)에서는 Dynamo 3.3~3.6에 PythonNet3 Engine 패키지를 설치하면 호환성을 유지할 수 있습니다. 

#### IronPython2에서 PythonNet3로 마이그레이션<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

그래프가 IronPython 엔진을 사용하는 경우에는 자동 마이그레이션이 제공되지 않습니다. 

해당 IronPython 패키지가 설치되어 있으면 그래프는 정상적으로 실행됩니다. 패키지가 없으면, Workspace References 확장에서 패키지를 다운로드하라는 의존성 경고가 표시됩니다. 패키지를 다시 설치하면 IronPython을 계속 사용할 수 있습니다. 다만 IronPython은 수년간 업데이트되지 않았고, Dynamo에서도 이미 상당 기간 해당 엔진에 대한 적극적인 지원이 중단된 상태이므로, 향후 그래프의 안정적인 동작을 보장하기 위해 PythonNet3로 마이그레이션하는 것을 강력히 권장합니다. DynamoIronPython2.7 및 DynamoIronPython3는 Dynamo Package Manager에서 패키지로 계속 사용할 수 있지만, Dynamo 팀에서 더 이상 유지 관리하지 않습니다. 

이 경우 사용할 수 있는 마이그레이션 옵션은 Python 편집기 내에서 사용할 수 있는 마이그레이션 보조 도구를 활용한 노드 단위 마이그레이션입니다.  

마이그레이션에 대한 자세한 내용은 이 [블로그](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)에서 확인할 수 있습니다.