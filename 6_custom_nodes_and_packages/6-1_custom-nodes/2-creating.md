# 사용자 노드 작성

Dynamo에서는 사용자 노드를 작성하는 여러 가지 다른 방법을 제공합니다. 사용자 노드를 처음부터 새로 만들거나, 기존 그래프에서 만들거나, C#에서 명시적으로 만들 수 있습니다. 이 섹션에서는 기존 그래프에서 Dynamo UI의 사용자 노드를 만드는 방법을 다룹니다. 이 방법은 작업공간을 정리하고, 노드 시퀀스를 패키징하여 다른 위치에서 재사용할 수 있도록 하는 데 적합합니다.

## 연습: UV 매핑을 위한 사용자 노드

### I부: 그래프로 시작

아래 이미지에서는 UV 좌표를 사용하여 한 표면에서 다른 표면으로 점을 매핑합니다. 이 개념을 사용하여 XY 평면의 곡선을 참조하는 패널형 표면을 작성해 보겠습니다. 여기서는 패널화를 위한 쿼드 패널을 작성할 것입니다. 그러나 동일한 논리를 사용하여 UV 매핑을 통해 다양한 패널을 작성할 수 있습니다. 이렇게 하면 그래프 또는 다른 Dynamo 워크플로우에서 비슷한 프로세스를 보다 쉽게 반복할 수 있으므로 이는 사용자 노드 개발에 매우 유용한 방법입니다.

![](../images/6-1/2/customnodeforuvmappingptI-01.jpg)

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/6-1/2/UV-CustomNode.zip" %}

먼저 사용자 노드에 중첩할 그래프를 작성해 보겠습니다. 이 예에서는 UV 좌표를 사용하여 기준 표면에서 대상 표면으로 다각형을 매핑하는 그래프를 작성합니다. 이 UV 매핑 프로세스는 자주 사용하는 방법이므로 사용자 노드에 적합합니다. 표면 및 UV 공간에 대한 자세한 내용은 [표면](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/5-surfaces.md) 페이지를 참조하십시오. 전체 그래프는 위의 다운로드한 .zip 파일에 포함된 _UVmapping_Custom-Node.dyn_ 입니다.

![](../images/6-1/2/customnodeforuvmappingptI-02.jpg)

> 1. **Code Block:** 이 줄을 사용하여 -45와 45 사이의 숫자 10개 범위를 작성합니다. `45..45..#10;`
> 2. **Point.ByCoordinates:** **Code Block**의 출력을 'x' 및 'y' 입력에 연결하고 레이싱을 상호 참조로 설정합니다. 이제 점 그리드가 표시됩니다.
> 3. **Plane.ByOriginNormal:** _‘Point’_ 출력을 _‘origin’_ 입력에 연결하여 각 점에서 평면을 작성합니다. 기본 법선 벡터인 (0,0,1)이 사용됩니다.
> 4. **Rectangle.ByWidthLength:** 이전 단계의 평면을 _‘plane’_ 입력에 연결하고 값이 _10_ 인 **Code Block**을 사용하여 폭과 길이를 지정합니다.

이제 직사각형 그리드가 표시됩니다. UV 좌표를 사용하여 이러한 직사각형을 대상 표면에 매핑해 보겠습니다.

![](../images/6-1/2/customnodeforuvmappingptI-03.jpg)

> 1. **Polygon.Points:** 이전 단계의 **Rectangle.ByWidthLength** 출력을 _‘polygon’_ 입력에 연결하여 각 직사각형의 코너 점을 추출합니다. 이러한 점은 대상 표면에 매핑할 점입니다.
> 2. **Rectangle.ByWidthLength:** 값이 _100_ 인 **Code Block** 을 사용하여 직사각형의 폭과 길이를 지정합니다. 이는 기준 표면의 경계가 됩니다.
> 3. **Surface.ByPatch:** 이전 단계의 **Rectangle.ByWidthLength**를 _‘closedCurve’_ 입력에 연결하여 기준 표면을 작성합니다.
> 4. **Surface.UVParameterAtPoint:** **Polygon.Points** 노드의 _‘Point’_ 출력과 **Surface.ByPatch** 노드의 _‘Surface’_ 출력을 연결하여 각 점에서 UV 매개변수를 반환합니다.

기준 표면과 UV 좌표 세트가 있으므로 대상 표면을 가져와 표면 사이의 점을 매핑할 수 있습니다.

![](../images/6-1/2/customnodeforuvmappingptI-04.jpg)

> 1. **File Path:** 가져올 표면의 파일 경로를 선택합니다. 파일 유형은 .SAT여야 합니다. _"찾아보기..."_ 버튼을 클릭하고 위에서 다운로드한 .zip 파일의 _UVmapping_srf.sat_ 파일로 이동합니다.
> 2. **Geometry.ImportFromSAT:** 파일 경로를 연결하여 표면을 가져옵니다. 가져온 표면이 형상 미리보기에 표시됩니다.
> 3. **UV:** UV 매개변수 출력을 _UV.U_ 및 _UV.V_ 노드에 연결합니다.
> 4. **Surface.PointAtParameter:** 가져온 표면과 u 및 v 좌표를 연결합니다. 이제 대상 표면에 3D 점 그리드가 표시됩니다.

마지막 단계는 3D 점을 사용하여 직사각형 표면 패치를 구성하는 것입니다.

![](../images/6-1/2/customnodeforuvmappingptI-05.jpg)

> 1. **PolyCurve.ByPoints:** 표면의 점을 연결하여 점을 통해 polycurve를 구성합니다.
> 2. **Boolean:** **Boolean**을 작업공간에 추가하고 _‘connectLastToFirst’_ 입력에 연결한 다음, True로 전환하여 polycurve를 닫습니다. 이제 표면에 매핑된 직사각형이 표시됩니다.
> 3. **Surface.ByPatch:** polycurve를 _‘closedCurve’_ 입력에 연결하여 표면 패치를 구성합니다.

### 2부: 그래프에서 사용자 노드로

이제 노드의 입력 및 출력으로 사용할 항목을 생각하며 사용자 노드에 중첩할 노드를 선택해 보겠습니다. 직사각형뿐만 아니라 다각형을 매핑할 수 있도록 사용자 노드를 가능한 한 유연하게 매핑해고자 합니다.

다음 노드(Polygon.Points로 시작)를 선택하고 작업공간을 마우스 오른쪽 버튼으로 클릭한 다음, '사용자 노드 작성'을 선택합니다.

![](../images/6-1/2/customnodeforuvmappingptII-01.jpg)

사용자 노드 특성 대화상자에서 사용자 노드에 이름, 설명 및 카테고리를 지정합니다.

![](../images/6-1/2/customnodeforuvmappingptII-02.jpg)

> 1. 이름: MapPolygonsToSurface
> 2. 설명: 베이스에서 대상 표면으로 다각형 매핑
> 3. 애드온 범주: Geometry.Curve

사용자 노드에 따라 작업공간이 상당히 정리되었습니다. 입력과 출력의 이름이 원래 노드에 따라 지정되었습니다. 보다 알아보기 쉬운 이름을 갖도록 사용자 노드를 편집해 보겠습니다.

![](../images/6-1/2/customnodeforuvmappingptII-03.jpg)

사용자 노드를 두 번 클릭하여 편집합니다. 그러면 노드의 내부를 나타내는 노란색 배경의 작업공간이 열립니다.

![](../images/6-1/2/customnodeforuvmappingptII-04.jpg)

> 1. **Inputs:** 입력 이름을 _baseSurface_ 및 _targetSurface_ 로 변경합니다.
> 2. **Outputs:** 매핑된 다각형에 출력을 추가합니다.

사용자 노드를 저장하고 홈 작업공간으로 돌아갑니다. **MapPolygonsToSurface** 노드에 방금 변경한 사항이 반영됩니다.

![](../images/6-1/2/customnodeforuvmappingptII-05.jpg)

**사용자 해설**에서 추가하여 사용자 노드의 견고성을 늘릴 수도 있습니다. 해설은 입력 및 출력 유형에서 힌트를 제공하거나 노드의 기능을 설명하는 데 도움이 될 수 있습니다. 사용자 노드의 입력 또는 출력 위에 마우스를 놓으면 해설이 나타납니다.

사용자 노드를 두 번 클릭하여 편집합니다. 그러면 노란색 배경 작업공간이 다시 열립니다.

![](../images/6-1/2/customnodeforuvmappingptII-06.jpg)

> 1. 입력 **Code Block** 편집을 시작합니다. 해설을 시작하려면 "//" 다음에 해설 문자를 입력합니다. 노드를 명확히 하는 데 도움이 될 수 있는 모든 항목을 입력합니다. 여기서는 _targetSurface_ 에 대해 설명합니다.
> 2. 또한 입력 유형을 값과 같게 설정하여 _inputSurface_ 의 기본값을 설정해 보겠습니다. 여기서는 기본값을 원래 **Surface.ByPatch** 세트로 설정합니다.

해설을 출력에 적용할 수도 있습니다.

![](../images/6-1/2/customnodeforuvmappingptII-07.jpg)

> 출력 Code Block에서 문자를 편집합니다. "//" 다음에 주석 문자를 입력합니다. 여기서는 보다 자세한 세부 설명을 추가하여 _Polygons_ 및 _surfacePatches_ 출력을 명확히 해 보겠습니다.

![](../images/6-1/2/customnodeforuvmappingptII-08.jpg)

> 1. 사용자 노드 입력 위에 커서를 놓으면 해설이 표시됩니다.
> 2. _inputSurface_ 에 기본값을 설정한 상태로, 표면 입력 없이 정의를 실행할 수도 있습니다.
