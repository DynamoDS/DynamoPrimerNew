# Zero-Touch로 한 단계 더 나아가기

Zero-Touch 프로젝트를 생성하는 방법을 이해했다면 Dynamo Github의 ZeroTouchEssentials 예를 통해 노드를 만드는 방법에 대한 세부 사항을 파악할 수 있습니다.

![Zero-Touch 노드](images/ootbzerotouch.png)

> 위에 나와 있는 대부분의 Math, Color 및 DateTime 노드와 마찬가지로 Dynamo의 표준 노드 대부분은 기본적으로 Zero-Touch 노드입니다.

시작하려면 [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials)에서 ZeroTouchEssentials 프로젝트를 다운로드하십시오.

Visual Studio에서 `ZeroTouchEssentials.sln` 솔루션 파일을 열고 솔루션을 빌드합니다.

![Visual Studio의 ZeroTouchEssentials](images/vs-build-zte.jpg)

> `ZeroTouchEssentials.cs` 파일에는 Dynamo로 가져올 모든 메서드가 포함되어 있습니다.

다음 예제에서 참조할 노드를 가져오기 위해 Dynamo를 열고 `ZeroTouchEssentials.dll`을 가져옵니다.

코드 예제는 [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs)에서 가져온 것으로, 일반적으로 ZeroTouchEssentials.cs와 일치합니다. 간결함을 유지하기 위해 XML 문서가 제거되었으며 각 코드 예는 위 이미지에 있는 노드를 생성합니다.

### 기본 입력 값 <a href="#default-input-values" id="default-input-values"></a>

Dynamo는 노드의 입력 포트에 대한 기본값 정의를 지원합니다. 이러한 기본값은 포트에 연결이 없는 경우 노드에 제공됩니다. 기본값은 [C# 프로그래밍 가이드](https://msdn.microsoft.com/ko-kr/library/dd264739.aspx)에서 선택적 인수를 지정하는 C# 메커니즘을 사용하여 표현됩니다. 기본값은 다음과 같은 방식으로 지정됩니다.

* 메서드 매개변수를 기본값인 `inputNumber = 2.0`으로 설정합니다.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![기본값](images/defaultval.jpg)

> 1. 노드 입력 포트 위에 마우스 포인터를 놓으면 기본값이 표시됩니다.

### 여러 값 반환하기 <a href="#returning-multiple-values" id="returning-multiple-values"></a>

여러 값을 반환하는 것은 여러 입력을 생성하는 것보다 조금 더 복잡하며 사전을 사용하여 반환해야 합니다. 사전의 항목은 노드의 출력 쪽에 있는 포트가 됩니다. 다음과 같은 방법으로 여러 개의 반환 포트가 생성됩니다.

* `Dictionary<>`를 사용하려면 `using System.Collections.Generic;`을 추가합니다.
* `MultiReturn` 속성을 사용하려면 `using Autodesk.DesignScript.Runtime;`을 추가합니다. 이것은 DynamoServices NuGet 패키지의 "DynamoServices.dll"을 참조합니다.
* 메서드에 `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` 속성을 추가합니다. 문자열은 사전의 키를 참조하며 출력 포트 이름이 됩니다.
* 속성의 매개변수 이름과 일치하는 키가 있는 함수 `return new Dictionary<string, object>`에서 `Dictionary<>`를 반환합니다.

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70)에서 이 코드 예를 참조하십시오.

여러 출력을 반환하는 노드.

![여러 출력](images/multipleoutputs.png)

> 1. 이제 사전의 키에 입력한 문자열에 따라 이름이 지정된 두 개의 출력 포트가 있음을 알 수 있습니다.

### 문서, 툴팁 및 검색 <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

노드의 함수, 입력, 출력, 검색 태그 등을 설명하는 문서를 Dynamo 노드에 추가하는 것이 좋습니다. 이 작업은 XML 문서 태그를 통해 수행되고, XML 문서는 다음과 같은 방법으로 생성됩니다.

* 슬래시 3개가 앞에 오는 설명 텍스트는 문서로 간주됩니다.
  * 예: `/// Documentation text and XML goes here`
* 슬래시 세 개 뒤에 Dynamo가 .dll을 가져올 때 읽을 메서드 위에 XML 태그를 생성합니다.
  * 예: `/// <summary>...</summary>`
* `Project > [Project] Properties > Build > Output`을 선택하고 `Documentation file`을 확인하여 Visual Studio에서 XML 문서를 활성화합니다.

![XML 파일 생성하기](images/vs-xml.jpg)

> 1. Visual Studio에서 지정된 위치에 XML 파일을 생성합니다.

태그 유형은 다음과 같습니다.

* `/// <summary>...</summary>`는 노드의 기본 문서이며 왼쪽 검색 사이드바의 노드 위에 툴팁으로 표시됩니다.
* `/// <param name="inputName">...</param>`은 특정 입력 매개변수에 대한 문서를 작성합니다.
* `/// <returns>...</returns>`는 출력 매개변수에 대한 문서를 생성합니다.
* `/// <returns name = "outputName">...</returns>`는 여러 출력 매개변수에 대한 문서를 생성합니다.
* `/// <search>...</search>`는 쉼표로 구분된 목록을 기반으로 검색 결과와 노드를 일치시킵니다. 예를 들어, 메쉬를 구획화하는 노드를 만드는 경우 "mesh", "subdivision" 및 "catmull-clark"와 같은 태그를 추가할 수 있습니다.

다음은 입력 및 출력 설명과 라이브러리에 표시되는 요약이 포함된 노드 예입니다.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80)에서 이 코드 예를 참조하십시오.

이 노드 예의 코드에는 다음이 포함되어 있습니다.

> 1. 노드 요약
> 2. 입력 설명
> 3. 출력 설명

#### Dynamo 노드 설명 모범 사례 

노드 설명은 노드의 기능과 출력을 간략하게 설명합니다. Dynamo에서는 노드 설명이 다음 두 곳에 나타납니다.

- 노드 툴팁
- 문서 탐색기

![노드 설명](images/node-description.png)

아래의 지침을 따르면 노드 설명을 작성하거나 업데이트할 때 일관성을 유지하고 시간을 절약할 수 있습니다.

##### 개요

설명은 한두 문장으로 작성해야 합니다. 더 많은 정보가 필요할 경우 문서 탐색기의 '상세' 아래에 추가합니다.

문장의 첫 번째 단어와 고유 명사는 대문자로 씁니다. 문장 끝에 마침표를 사용하지 않습니다.

표현은 가능한 한 명확하고 단순해야 합니다. 약어는 전문가가 아닌 사용자도 알 수 있을 정도로 잘 알려져 있지 않은 경우 처음 사용 시 정의합니다.

항상 명확성을 우선시합니다. 이로 인해 이러한 지침에서 벗어나더라도 명확성을 우선시해야 합니다.

##### 지침

| 해야 할 일(영문 기준)      | 하지 말아야 일(영문 기준) |
| ----------- | ----------- |
| 3인칭 동사로 설명을 시작합니다. <ul><li>예: *Determines* if one geometry object intersects with another</li></ul>      | 2인칭 동사나 명사로 시작하지 마십시오. <ul><li>예: *Determine* if one geometry object intersects with another</li></ul>       |
| "Gets" 대신 "Returns", "Creates" 또는 다른 설명 동사를 사용합니다. <ul><li>예: *Returns* a Nurbs representation of a surface</li></ul>   | "Get" 또는 "Gets"를 사용하지 마십시오. "Get" 또는 "Gets"는 덜 구체적이고 다양한 의미로 번역될 수 있습니다. <ul><li>예: *Gets* a Nurbs representation of the surface</li></ul>        |
| 입력을 언급할 때는 "specified" 또는 다른 용어 대신 "given" 또는 "input"을 사용합니다. 가능하면 "given" 또는 "input"을 생략하여 설명을 단순화하고 단어 수를 줄입니다. <ul><li>예: Deletes the *given* file</li><li>예: Projects a curve along the *given* projection direction onto *given* base geometry</li></ul>입력을 직접 언급하지 않을 때는 "specified"를 사용해도 됩니다. <ul><li>예: Writes text content to a file *specified* by the given path</li></ul>       | 입력을 언급할 때 일관성을 유지하기 위해 "specified" 또는 다른 용어는 사용하지 말고 "given" 또는 "input"만 사용합니다. 명확성을 위해 필요한 경우를 제외하고 동일한 설명에서 "given"과 "input"을 혼용하지 마십시오. <ul><li>예: Deletes the *specified* file</li><li>예: Projects an *input* curve along a *given* projection direction onto a *specified* base geometry</li></ul>      |
| 입력을 처음 언급할 때는 "a" 또는 "an"을 사용합니다. 명확성을 위해 필요에 따라 "a" 또는 "an" 대신 "the given" 또는 "the input"을 사용합니다.<ul><li>예: Sweeps *a* curve along the path curve</li></ul>      | 입력을 처음 언급할 때 "this"를 사용하지 마십시오. <ul><li>예: Sweeps *this* curve along the path curve      |
| 노드 작업의 대상인 출력 또는 기타 명사를 처음 언급할 때 "a" 또는 "an"을 사용합니다. "the"는 "input" 또는 "given"과 짝을 이룰 때만 사용합니다. <ul><li>예: Copies *a* file</li><li>예: Copies *the given* file</li></ul>      | 노드 작업의 대상인 출력 또는 다른 명사를 처음 언급할 때 "the"만 단독으로 사용하지 마십시오. <ul><li>예: Copies *the* file</li></ul>      |
| 문장의 첫 번째 단어와 이름과 같은 고유명사 및 전통적으로 대문자로 쓰는 명사는 대문자로 씁니다. <ul><li>예: Returns the intersection of two *BoundingBoxes*</li></ul>      | 일반적인 형상 객체 및 개념은 명확성을 위해 필요한 경우가 아니면 대문자로 쓰지 마십시오. <ul><li>예: Scales non-uniformly around the given *Plane*      |
| 부울을 대문자로 씁니다. 부울의 출력을 언급할 때는 True 및 False를 대문자로 씁니다. <ul><li>예: Returns *True* if the two values are different</li><li>예: Converts a string to all uppercase or all lowercase characters based on a *Boolean* parameter      | 부울을 소문자로 쓰지 마십시오. 부울의 출력을 언급할 때 True 및 False를 소문자로 쓰지 마십시오. <ul><li>예: Returns *true* if the two values are different</li><li>예: Converts a string to all uppercase characters or all lowercase characters based on a *boolean* parameter</li></ul>

#### Dynamo 노드 경고 및 오류

노드 경고 및 오류 경고는 그래프 관련 문제를 사용자에게 알립니다. 노드 위에 아이콘과 확장된 텍스트 버블을 표시하여 정상적인 그래프 작동을 방해하는 문제를 사용자에게 알립니다. 노드 오류 및 경고의 심각도는 다양할 수 있습니다. 일부 그래프는 경고가 발생해도 충분히 실행할 수 있는 반면 다른 그래프는 예상 결과를 차단합니다. 어떤 경우든 노드 오류 및 경고는 그래프 관련 문제를 사용자에게 알리기 위한 중요한 도구입니다.

노드 경고 및 오류 메시지를 작성하거나 업데이트할 때 일관성을 유지하고 시간을 절약하는 데 도움이 되는 지침은 [컨텐츠 패턴: 노드 경고 및 오류](https://github.com/DynamoDS/Dynamo/wiki/Content-Pattern:-Node-Warnings-and-Errors) Wiki 페이지를 참조하십시오.

### 객체 <a href="#objects" id="objects"></a>

Dynamo에는 `new` 키워드가 없으므로 정적 구성 방법을 사용하여 객체를 구성해야 합니다. 객체는 다음과 같은 방식으로 구성됩니다.

* 다른 요구 사항이 없는 한 생성자를 내부 `internal ZeroTouchEssentials()`로 만듭니다.
* `public static ZeroTouchEssentials ByTwoDoubles(a, b)`와 같은 정적 메서드로 객체를 구성합니다.

> 참고: Dynamo는 정적 메서드가 생성자임을 나타내는 접두사 "By"를 사용합니다. 선택 사항이지만 "By"를 사용하면 라이브러리를 기존 Dynamo 스타일에 더 잘 맞도록 할 수 있습니다.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26)에서 이 코드 예를 참조하십시오.

ZeroTouchEssentials dll을 가져오면 라이브러리에 ZeroTouchEssentials 노드가 생성됩니다. 이 객체는 `ByTwoDoubles` 노드를 사용하여 생성할 수 있습니다.

![ByTwoDoubles 노드](images/dyn-constructor.jpg)

### Dynamo 형상 유형 사용하기 <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Dynamo 라이브러리는 기본 Dynamo 형상 유형을 입력으로 사용하고 새 형상을 출력으로 생성할 수 있습니다. 형상 유형은 다음과 같은 방식으로 생성됩니다.

* C# 파일의 맨 위에 `using Autodesk.DesignScript.Geometry;`를 포함하고 프로젝트에 ZeroTouchLibrary NuGet 패키지를 추가하여 프로젝트에서 "ProtoGeometry.dll"을 참조합니다.
* **중요:** 함수에서 반환되지 않는 형상 리소스를 관리하려면 아래의 **Dispose/using 문** 섹션을 참조하십시오.

> 참고: Dynamo 형상 객체는 함수에 전달된 다른 모든 객체처럼 사용됩니다.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86)에서 이 코드 예를 참조하십시오.

곡선의 길이를 가져와서 두 배로 늘리는 노드

![곡선 입력](images/doublelength.png)

> 1. 이 노드는 곡선 형상 유형을 입력으로 허용합니다.

### Dispose/using 문 <a href="#disposeusing-statements" id="disposeusing-statements"></a>

함수에서 반환되지 않는 형상 리소스는 Dynamo 버전 2.5 이상을 사용하지 않는 한 수동으로 관리해야 합니다. Dynamo 2.5 이상 버전에서는 형상 리소스가 시스템 내부에서 처리되지만 복잡한 사용 사례가 있거나 결정적인 시간에 메모리를 줄여야 하는 경우 형상을 수동으로 제거해야 할 수 있습니다. Dynamo 엔진은 함수에서 반환되는 모든 형상 리소스를 처리합니다. 반환되지 않는 형상 리소스는 다음과 같은 방법으로 수동으로 처리할 수 있습니다.

*   using 문:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > using 문은 [여기](https://msdn.microsoft.com/ko-kr/library/yh598w02.aspx)에 설명되어 있습니다.
    >
    > Dynamo 2.5에 추가된 새로운 안정성 기능에 대한 자세한 내용은 [Dynamo 형상 안정성 개선 사항](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297)을 참조하십시오.
*   수동 Dispose 호출:

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

### 마이그레이션 <a href="#migrations" id="migrations"></a>

최신 버전의 라이브러리를 게시하는 경우 노드 이름이 변경될 수 있습니다. 마이그레이션 파일에 이름 변경 사항을 지정하여 이전 버전의 라이브러리에서 작성된 그래프가 업데이트 시에도 계속 제대로 작동하도록 할 수 있습니다. 마이그레이션은 다음과 같은 방식으로 구현됩니다.

* `.dll`과 동일한 폴더에 "BaseDLLName".Migrations.xml 형식의 `.xml` 파일을 생성합니다.
* `.xml`에서 하나의 `<migrations>...</migrations>` 요소를 생성합니다.
* 이름을 변경할 때마다 마이그레이션 요소 내부에서 `<priorNameHint>...</priorNameHint>` 요소를 생성합니다.
* 이름을 변경할 때마다 `<oldName>...</oldName>` 및 `<newName>...</newName>` 요소를 제공합니다.

![마이그레이션 파일](images/vs-migrations-file.jpg)

> 1. `Add > New Item`을 마우스 오른쪽 버튼으로 클릭하여 선택합니다.
> 2. `XML File`을 선택합니다.
> 3. 이 프로젝트의 경우 마이그레이션 파일의 이름을 `ZeroTouchEssentials.Migrations.xml`로 지정합니다.

이 코드 예는 `GetClosestPoint`라는 이름의 모든 노드가 이제 `ClosestPointTo`라는 이름으로 지정되었다는 것을 Dynamo에 알려줍니다.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml)의 이 코드 예를 참조하십시오.

### 제네릭 <a href="#generics" id="generics"></a>

Zero-Touch는 현재 제네릭 사용을 지원하지 않습니다. 제네릭을 사용할 수는 있지만, 유형이 설정되지 않은 경우 직접 가져온 코드에서는 사용할 수 없습니다. 제네릭에 해당하고 유형이 설정되어 있지 않은 메서드, 특성 또는 클래스는 노출할 수 없습니다.

아래 예에서는 `T` 유형의 Zero-Touch 노드를 가져오지 않습니다. 라이브러리의 나머지 부분을 Dynamo로 가져오면 유형 예외가 누락됩니다.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

이 예에서 설정된 유형과 함께 제네릭 유형을 사용하면 Dynamo로 가져올 수 있습니다.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
