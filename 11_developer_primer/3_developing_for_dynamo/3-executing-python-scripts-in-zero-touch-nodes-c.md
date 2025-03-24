# Zero-Touch 노드에서 Python 스크립트 실행하기(C#)

### Zero-Touch 노드에서 Python 스크립트 실행하기(C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Python에서 스크립트를 작성하는 데 익숙하고 표준 Dynamo Python 노드에서 더 많은 기능을 사용하고자 하는 경우 Zero-Touch를 사용하여 자체 노드를 만들 수 있습니다. Python 스크립트를 문자열로 Zero-Touch 노드에 전달하여 스크립트가 실행되고 결과가 반환되는 간단한 예로 시작하겠습니다. 이 사례 연구는 연습과 시작하기 섹션의 예제를 기반으로 합니다. Zero-Touch 노드를 만드는 것이 완전히 처음인 경우 해당 섹션을 참조하십시오.

![Python 스크립트 문자열을 실행할 Zero-Touch 노드](images/python-case-study.png)

> Python 스크립트 문자열을 실행할 Zero-Touch 노드

#### Python 엔진 <a href="#python-engine" id="python-engine"></a>

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

Python 스크립트가 변수 `output`을 반환하므로 Python 스크립트에 `output` 변수가 있어야 합니다. 이 샘플 스크립트를 사용하여 Dynamo의 노드를 테스트하십시오. Dynamo에서 Python 노드를 사용해 본 적이 있다면 다음이 익숙할 것입니다. 자세한 내용은 [입문서의 Python 섹션](https://primer2.dynamobim.org/v/ko/8_coding_in_dynamo/8-3_python)을 참조하십시오.

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

![이 노드를 사용하면 직육면체의 체적과 중심을 모두 반환할 수 있습니다.](images/python-multi-case-study.png)

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
