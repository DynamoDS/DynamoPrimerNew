# Python 및 Civil 3D

Dynamo는 [시각적 프로그래밍](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) 도구로서 매우 강력하지만 노드 및 와이어를 넘어 문자 형식으로 코드를 작성하는 것도 가능합니다. 이 작업은 다음과 같은 두 가지 방법으로 수행할 수 있습니다.

1. Code Block을 사용하여 **DesignScript** 쓰기
2. Python 노드를 사용하여 **Python** 쓰기

이 섹션에서는 Civil 3D 환경에서 Python을 활용하여 AutoCAD 및 Civil 3D .NET API를 활용하는 방법에 대해 중점적으로 설명합니다.

{% hint style="info" %} Dynamo에서 Python을 사용하는 방법에 대한 보다 일반적인 정보는 [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") 섹션을 참조하십시오. {% endhint %}

## API 문서

AutoCAD와 Civil 3D에는 여러분과 같은 개발자가 사용자 지정 기능으로 핵심 제품을 확장할 수 있는 여러 API가 있습니다. Dynamo의 컨텍스트에서 관련성이 있는 것은 **관리되는 .NET API**입니다. 다음 링크는 API의 구조 및 작동 방식을 이해하는 데 필수적입니다.

[AutoCAD .NET API 개발자 안내서](https://help.autodesk.com/view/OARX/2024/KOR/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[AutoCAD .NET API 참조 안내서](https://help.autodesk.com/view/OARX/2024/KOR/?guid=OARX-ManagedRefGuide-What_s_New)

[Civil 3D .NET API 개발자 안내서](https://help.autodesk.com/view/CIV3D/2024/KOR/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Civil 3D .NET API 참조 안내서](https://help.autodesk.com/view/CIV3D/2024/KOR/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} 이 섹션을 진행하면서 데이터베이스, 트랜잭션, 메서드, 특성 등 익숙하지 않은 개념이 접할 수 있습니다. 이러한 개념 중 대부분은 .NET API 작업의 핵심이며 Dynamo 또는 Python에만 국한되지 않습니다. 이러한 항목에 대해 자세히 설명하는 것은 이 입문서 섹션의 범위를 벗어나는 것이므로 더 자세한 내용은 위의 링크를 자주 참조하는 것이 좋습니다. {% endhint %}

## 코드 템플릿

새 Python 노드를 처음 편집하면 시작할 수 있도록 템플릿 코드가 미리 채워져 있습니다. 아래에는 각 블록에 대한 설명과 함께 템플릿을 분석한 내용이 나와 있습니다.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Civil 3D의 기본 Python 템플릿</p></figcaption></figure>

> 1. `sys` 및 `clr` 모듈을 가져옵니다. 둘 다 Python 인터프리터가 제대로 작동하는 데 필요합니다. 특히 `clr` 모듈을 사용하면 .NET 네임스페이스를 기본적으로 Python 패키지로 처리할 수 있습니다.
> 2. AutoCAD 및 Civil 3D를 위한 관리되는 .NET API로 작업하기 위한 표준 어셈블리(예: DLL)를 로드합니다.
> 3. 표준 AutoCAD 및 Civil 3D 네임스페이스에 참조를 추가합니다. 이는 각각 C# 또는 VB.NET의 `using` 또는 `Imports` 지시어와 동일합니다.
> 4. 노드의 입력 포트에는 `IN`라는 사전 정의된 리스트를 사용하여 액세스할 수 있습니다. 색인 번호를 사용하여 특정 포트의 데이터에 액세스할 수 있습니다(예: `dataInFirstPort = IN[0]`).
> 5. 활성 문서 및 편집기를 가져옵니다.
> 6. 문서를 잠그고 데이터베이스 트랜잭션을 시작합니다.
> 7. 여기에 스크립트 논리의 대부분을 배치해야 합니다.
> 8. 주 작업이 완료된 후 이 행의 주석을 해제하여 트랜잭션을 커밋합니다.
> 9. 노드에서 데이터를 출력하려면 스크립트 끝에 있는 `OUT` 변수에 데이터를 지정합니다.

{% hint style="info" %} **사용자화하시겠습니까?**\
 `C:\ProgramData\Autodesk\C3D <version>\Dynamo` 에 있는 `PythonTemplate.py` 파일을 편집하여 기본 Python 템플릿을 수정할 수 있습니다. {% endhint %}

## 예제

예제를 통해 Dynamo for Civil 3D에서 Python 스크립트를 작성하는 데 필요한 몇 가지 기본 개념을 살펴보겠습니다.

### 목표

> :dart: 도면에 있는 모든 유역의 경계 형상을 가져옵니다.

### 데이터세트

다음은 이 연습에서 참조할 수 있는 예제 파일입니다.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### 솔루션 개요

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. Civil 3D API 문서 검토
> 2. 문서에서 도면층 이름별로 모든 유역 선택
> 3. Dynamo 객체를 "언래핑"하여 내부 Civil 3D API 멤버에 액세스
> 4. AutoCAD 점에서 Dynamo 점 작성
> 5. 점에서 PolyCurve 작성

그럼 시작하겠습니다!

### API 설명서 검토

그래프 작성과 코드 작성을 시작하기 전에 Civil 3D API 문서를 살펴보고 API가 제공하는 기능을 파악하는 것이 좋습니다. 이 경우 유역의 경계 점을 반환하는 [특성이 유역 클래스](https://help.autodesk.com/view/CIV3D/2024/KOR/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c)에 있습니다. 이 특성은 `Point3dCollection` 객체를 반환하는데, Dynamo에서 이 객체를 어떻게 사용할지 알 수 없습니다. 다시 말해, `Point3dCollection`에서 PolyCurve를 작성할 수 없으므로, 결국 모든 항목을 Dynamo 점으로 변환해야 합니다. 이에 대해서는 나중에 더 자세히 살펴보겠습니다.

### 모든 유역 가져오기

이제 그래프 논리 작성을 시작할 수 있습니다. 가장 먼저 해야 할 일은 문서에 있는 모든 유역 리스트를 가져오는 것입니다. 이를 위해 사용할 수 있는 노드가 있으므로 Python 스크립트에 포함할 필요가 없습니다. 노드를 사용하면 그래프를 읽을 수 있는 다른 사람에게 더 나은 가시성을 제공할 수 있습니다(Python 스크립트에 많은 코드를 묻어두는 것에 비해). 또한 Python 스크립트가 유역의 경계 점을 반환하는 한 가지 작업에 집중할 수 있도록 해줍니다.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>문서에서 도면층별로 모든 유역 가져오기</p></figcaption></figure>

여기서 **All Objects on Layer** 노드의 출력은 CivilObjects 목록입니다. 이는 현재 Dynamo for Civil 3D에 유역 작업을 위한 노드가 없기 때문이며, 이것이 바로 Python을 통해 API에 액세스해야 하는 이유입니다.

### 객체 언래핑

더 자세히 알아보기 전에 중요한 개념을 간단히 살펴봐야 합니다. [node-library.md](../node-library.md "mention") 섹션에서 객체와 CivilObjects가 어떻게 관련되어 있는지 살펴보았습니다. 이에 대해 조금 더 자세히 설명하자면, **Dynamo 객체** 는 **AutoCAD 도면요소** 를 감싸는 래퍼입니다. 마찬가지로, **Dynamo CivilObject** 도 **Civil 3D 도면요소** 를 감싸는 래퍼입니다. 해당 `InternalDBObject` 또는 `InternalObjectId` 특성에 액세스하여 객체의 "언래핑"할 수 있습니다.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Dynamo 유형</th><th width="373">랩</th></tr></thead><tbody><tr><td><strong>객체</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>도면요소</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>도면요소</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} 경험상 일반적으로 `InternalObjectId` 특성을 사용하여 객체 ID를 가져온 다음 트랜잭션에서 래핑된 객체에 액세스하는 것이 더 안전합니다. 이는 `InternalDBObject` 특성이 쓰기 가능 상태가 아닌 AutoCAD DBObject를 반환하기 때문입니다. {% endhint %}

### Python 스크립트

다음은 내부 유역 객체에 액세스하는 작업을 수행하는 완전한 Python 스크립트가 해당 경계 점을 가져오는 것입니다. 강조 표시된 행은 기본 템플릿 코드에서 수정/추가된 행을 나타냅니다.

{% hint style="info" %} 각 행에 대한 설명을 보려면 스크립트에서 밑줄이 있는 문자를 클릭합니다. {% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Load the Python Standard and DesignScript Libraries
import sys
import clr

# Add Assemblies for AutoCAD and Civil3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Import references from AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Import references from Civil3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# The inputs to this node will be stored as a list in the IN variables.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Commit before end transaction
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Assign your output to the OUT variable.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} 경험상 스크립트 논리의 대부분은 트랜잭션 내에 포함하는 것이 가장 좋습니다. 이렇게 하면 스크립트가 읽기/쓰기 중인 객체에 안전하게 액세스할 수 있습니다. 대부분의 경우 트랜잭션을 생략하면 치명적 오류가 발생할 수 있습니다. {% endhint %}

### PolyCurve 만들기

이 단계에서 Python 스크립트는 배경 미리보기에서 볼 수 있는 Dynamo 점 리스트를 출력해야 합니다. 마지막 단계는 점에서 PolyCurve를 만드는 것입니다. 이 작업은 Python 스크립트에서 직접 수행할 수도 있지만, 더 잘 보이도록 의도적으로 스크립트 외부의 노드에 배치했습니다. 최종 그래프의 모습은 다음과 같습니다.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>최종 그래프</p></figcaption></figure>

### 결과

최종 Dynamo 형상은 다음과 같습니다.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>유역 경계에 대한 Dynamo PolyCurve 결과물</p></figcaption></figure>

> :tada: 작업을 완료했습니다!

## IronPython과 CPython

마무리하기 전에 간단히 한 가지 더 알려드리겠습니다. 사용 중인 Civil 3D 버전에 따라 Python 노드가 다르게 구성될 수 있습니다. **Civil 3D 2020 및 2021** 에서 Dynamo는 **IronPython** 이라는 도구를 사용하여 .NET 객체와 Python 스크립트 간에 데이터를 이동했습니다. 그러나 **Civil 3D 2022** 에서는 Dynamo가 Python 3을 사용하는 대신 표준 기본 Python 인터프리터(**CPython**)를 사용하도록 전환되었습니다. 이러한 전환의 이점으로는 널리 사용되는 최신 라이브러리와 새로운 플랫폼 기능, 필수 유지보수 및 보안 패치에 대한 액세스가 포함됩니다.

{% hint style="info" %} [Dynamo 블로그](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/) 에서 이 전환에 대한 자세한 내용과 기존 스크립트를 업그레이드하는 방법을 확인할 수 있습니다. IronPython을 계속 사용하려면 Dynamo Package Manager를 사용하여 **DynamoIronPython2.7** 패키지를 설치하기만 하면 됩니다. {% endhint %}

[^1]: 기본적으로 Dynamo 형상 라이브러리는 Python 환경에 추가되지 않습니다. 이 스크립트의 목적은 유역 경계에 대한 Dynamo 점 리스트를 출력하는 것이므로, 나중에 점을 작성하기 위해 이 행을 추가해야 합니다.

[^2]: 이 행은 Dynamo 형상 라이브러리에서 필요한 특정 클래스를 가져옵니다. `import *`는 명명 충돌을 유발할 수 있으므로, 여기서는 `import Point as DynPoint`를 지정합니다.

[^3]: 여기서는 기본 `dataEnteringNode` 변수의 이름을 더 잘 설명할 수 있도록 `objs`로 변경합니다.

[^4]: 여기서는 모든 입력의 전체 리스트를 참조하는 기본 `IN` 대신 원하는 데이터가 포함되어 있는 입력 포트를 정확하게 지정합니다.

[^5]: 이렇게 하면 나중에 출력 데이터를 추가할 빈 리스트가 생성됩니다.

[^6]: 이것은 스크립트에 대한 빈 입력 또는 null 입력이 발생할 가능상을 방지하기 위한 것입니다.

[^7]: 스크립트를 끊지 않고 스크립트를 계속할 수 없는 이유를 설명하는 유용한 메시지를 출력하겠습니다.

[^8]: 스크립트의 나머지 부분에서는 작업할 객체(단일 객체가 아님) 리스트가 있다고 가정합니다. 그러나 객체가 하나만 있는 경우(즉, 하나의 유역이 있는 도면)에도 스크립트를 실행할 수 있어야 합니다.

[^9]: 입력이 리스트가 아닌 경우(즉, 단일 객체) 객체를 유일한 항목으로 하는 새 리스트를 작성하기만 하면 됩니다.

[^10]: 리스트의 각 객체에 대해 루프를 시작합니다.

[^11]: 객체 ID를 가져와서 Dynamo 객체를 "언래핑"합니다.

[^12]: AutoCAD 데이터베이스에서 "래핑된" 객체를 검색합니다. 객체를 편집하지 않을 것이므로, 여기서 OpenMode는 `ForRead`로 설정되어 있습니다. 데이터를 "쿼리"하기만 하면 됩니다.

[^13]: 객체의 입력 리스트에 유역과 기타 비유역 항목이 혼합되어 있을 수 있습니다. 이 상황을 확인하고 적절히 처리해야 합니다(즉, 항목이 실제로 유역인 경우에만 이 루프 반복을 계속함).

[^14]: 스크립트가 여기까지 도달하면 객체가 실제로 유역임을 알 수 있습니다. 이름을 명확하고 이해하기 쉽게 만들기 위해 여기에 새 변수를 추가하겠습니다.

[^15]: 여기서는 앞서 API 문서에서 조회한 적절한 특성을 사용하여 유역 경계 점을 검색합니다. 앞에서 설명한 것처럼 이 옵션은 기본적으로 AutoCAD 점 리스트인 `Point3dCollection` 객체를 제공합니다. 이를 유용하게 사용할 수 있도록 Dynamo 점으로 "변환"해야 합니다.

[^16]: 이 유역의 경계 점을 저장할 빈 리스트를 작성합니다.

[^17]: `Point3dCollection`에서 각 `Point3d` 객체에 대한 루프를 시작합니다.

[^18]: AutoCAD 점의 좌표를 사용하여 Dynamo 점을 작성합니다.

[^19]: 리스트에 점을 추가합니다.

[^20]: 모든 AutoCAD 점을 Dynamo 점으로 "변환"하는 작업을 마치고 나면, 결과 리스트를 출력에 추가합니다. 그 후 외부 루프는 입력 리스트의 다음 객체로 이동합니다.

[^21]: 이 행의 주석을 해제하여 트랜잭션을 커밋합니다.

[^22]: 마지막으로 Dynamo 점 리스트를 출력합니다.
