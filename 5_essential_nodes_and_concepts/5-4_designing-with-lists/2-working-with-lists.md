# 리스트 작업

### 리스트 작업

지금까지 리스트가 무엇인지 확인했으므로 리스트에서 수행할 수 있는 작업에 대해 설명하겠습니다. 리스트가 하나의 게임 카드 세트라고 생각해 보십시오. 세트는 리스트이며, 각 카드는 하나의 항목을 나타냅니다.

![카드](../images/5-4/2/Playing\_cards\_modified.jpg)

> 사진 출처: [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing\_cards\_modified.jpg)

### 조회

리스트에서는 어떤 **조회**를 만들 수 있을까요? 이러한 조회에서는 기존 특성을 확인합니다.

* 한 세트에 들어 있는 카드의 수는? 52\.
* 짝을 이루는 패의 수는? 4\.
* 재료? 종이.
* 길이는? 3.5" 또는 89mm.
* 폭은? 2.5" 또는 64mm.

### 작업

리스트에서 수행할 수 있는 **작업**은 무엇일까요? 작업을 수행하면 해당 작업을 기준으로 리스트가 변경됩니다.

* 세트를 섞을 수 있습니다.
* 값별로 세트를 정렬할 수 있습니다.
* 짝을 이루는 패별로 세트를 정렬할 수 있습니다.
* 세트를 나눌 수 있습니다.
* 각 사람에게 카드를 나눠주기 위해 세트를 나눌 수 있습니다.
* 세트에서 특정 카드를 선택할 수 있습니다.

위에 나열된 모든 작업에는 일반 데이터 리스트로 작업할 수 있도록 비슷한 Dynamo 노드가 있습니다. 아래 단원에는 리스트에서 수행할 수 있는 기본적인 작업 중 일부가 나와 있습니다.

## **연습**

### **리스트 작업**

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

아래 이미지는 두 원 사이에 선을 그려 기본 리스트 작업을 표현하는 기준 그래프입니다. 리스트 내에서 데이터를 관리하는 방법을 살펴보고 아래의 리스트 작업을 통해 시각적 결과를 표시해 보겠습니다.

![](../images/5-4/2/workingwithlist-listoperation.jpg)

> 1. 값이 `500;`인 **Code Block**으로 시작합니다.
> 2. **Point.ByCoordinates** 노드의 x 입력에 연결합니다.
> 3. 이전 단계의 노드를 **Plane.ByOriginNormal** 노드의 원점 입력에 연결합니다.
> 4. **Circle.ByPlaneRadius** 노드를 사용하여 이전 단계의 노드를 평면 입력에 연결합니다.
> 5. **Code Block**을 사용하여 radius에 대해 값 `50;`을 지정합니다. 다음은 작성할 첫 번째 원입니다.
> 6. **Geometry.Translate** 노드를 사용하여 Z 방향으로 최대 100개 단위만큼 원을 이동합니다.
> 7. **Code Block** 노드에서 코드 줄 `0..1..#10;`을 사용하여 0과 1 사이의 10개 숫자 범위를 정의합니다.
> 8. 이전 단계의 code block을 두 **Curve.PointAtParameter** 노드의 _param_ 입력에 연결합니다. **Circle.ByPlaneRadius**를 최상위 노드의 곡선 입력에 연결하고 **Geometry.Translate**를 그 아래 노드의 곡선 입력에 연결합니다.
> 9. **Line.ByStartPointEndPoint**를 사용하여 두 **Curve.PointAtParamete**_r_ 노드를 연결합니다.

### List.Count

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-Count.dyn" %}

_List.Count_ 노드는 간단합니다. 리스트의 값 개수를 계산하고 해당 수를 반환합니다. 이 노드는 리스트의 리스트로 작업할 경우 미묘한 차이가 있으며, 이 내용은 다음 섹션에서 살펴보겠습니다.

![개수](../images/5-4/2/workingwithlist-listoperation-listcount.jpg)

> 1. **List.Count **_****_ 노드에서는 **Line.ByStartPointEndPoint** 노드의 줄 수를 반환합니다. 이 경우 해당 값은 원래 **Code Block** 노드에서 작성된 점 수와 일치하는 10입니다.

### List.GetItemAtIndex

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

**List.GetItemAtIndex**는 기본적으로 리스트의 항목을 조회하는 기본적인 방법입니다.

![연습](../images/5-4/2/workingwithlist-getitemindex01.jpg)

> 1. 먼저 **Line.ByStartPointEndPoint** 노드를 마우스 오른쪽 버튼으로 클릭하여 해당 미리보기를 끕니다.
> 2. **List.GetItemAtIndex** 노드를 사용하여 색인 _"0"_ 또는 줄 리스트의 첫 번째 항목을 선택합니다.

슬라이더 값을 0에서 9 사이로 변경하여 **List.GetItemAtIndex**를 통해 다른 항목을 선택합니다.

![](../images/5-4/2/workingwithlist-getitemindex02.gif)

### List.Reverse

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

_List.Reverse_ 는 리스트의 모든 항목 순서를 반대로 합니다.

![연습](../images/5-4/2/workingwithlist-listreverse.jpg)

> 1. 반전된 줄 리스트를 올바르게 시각화하려면 **Code Block**을 `0..1..#50;`으로 변경하여 더 많은 줄을 작성합니다.
> 2. **Line.ByStartPointEndPoint** 노드를 복제하고 **Curve.PointAtParameter** 및 두 번째 **Line.ByStartPointEndPoint** 사이에 List.Reverse 노드를 삽입합니다.
> 3. **Watch3D** 노드를 사용하여 두 가지 다른 결과를 미리 봅니다. 첫 번째 항목에서는 반전된 리스트가 없는 결과를 보여 줍니다. 줄은 인접한 점에 수직으로 연결됩니다. 그러나 반전된 리스트에서는 모든 점을 다른 리스트에 반대 순서로 연결합니다.

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

**List.ShiftIndices**는 틀기 또는 나선형 패턴이나 기타 유사한 데이터 조작을 작성하는 데 유용한 도구입니다. 이 노드에서는 리스트의 항목을 지정된 색인만큼 이동합니다.

![연습](../images/5-4/2/workingwithlist-shiftIndices01.jpg)

> 1. 반전 리스트와 동일한 프로세스에서 **Curve.PointAtParameter** 및 **Line.ByStartPointEndPoint**에 **List.ShiftIndices**를 삽입합니다.
> 2. **Code Block**을 사용하여 리스트를 1개 색인만큼 이동하기 위해 값 "1"을 지정했습니다.
> 3. 변경된 정도는 미세하지만 다른 점 세트에 연결할 때 하단 **Watch3D** 노드의 모든 줄이 1개 색인만큼 이동되었습니다.

예를 들어 **Code Block**을 더 큰 값인 _"30"_ 으로 변경하면 대각선이 크게 달라진다는 것을 알 수 있습니다. 이 경우 이동은 카메라의 홍채처럼 작동하여 원래의 원통형 형식에서 틀기가 작성됩니다.

![](../images/5-4/2/workingwithlist-shiftIndices02.jpg)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

**List.FilterByBooleanMask**는 부울 리스트나 "true" 또는 "false" 값을 기준으로 특정 항목을 제거합니다.

![연습](../images/5-4/2/workingwithlist-filterbyboolmask.jpg)

"true" 또는 "false" 값 리스트를 작성하려면 좀 더 많은 작업이 필요합니다.

> 1. **Code Block**을 사용하여 `0..List.Count(list);` 구문으로 표현식을 정의합니다. **Curve.PointAtParameter** 노드를 _list_ 입력에 연결합니다. code block록 장에서 이 설정에 대해 좀 더 자세히 설명하겠지만, 이 경우에는 코드 줄에서 **Curve.PointAtParameter** 노드의 각 색인을 나타내는 리스트를 제공합니다.
> 2. _**%**_** (modulus)** 노드를 사용하여 _code block_ 의 출력을 _x_ 입력에 연결하고 _4_ 값을 _y_ 입력에 연결합니다. 이렇게 하면 색인 리스트를 4로 나눈 나머지가 표시됩니다. Modulus는 패턴 작성에 매우 유용한 노드입니다. 모든 값은 가능한 나머지 4개(0, 1, 2, 3)로 표시됩니다.
> 3. _**%**_** (modulus)** 노드에서 0 값은 색인이 4로 나누어떨어짐을 의미합니다(0, 4, 8 등). **==** 노드를 사용하면 _"0"_ 값을 기준으로 테스트하여 나누어떨어지는지 테스트할 수 있습니다.
> 4. **Watch** 노드에서는 _true,false,false,false..._ 로 표시되는 true/false 패턴이 있음을 나타냅니다.
> 5. 이 true/false 패턴을 사용하여 두 **List.FilterByBooleanMask** 노드의 마스크 입력에 연결합니다.
> 6. **Curve.PointAtParameter** 노드를 **List.FilterByBooleanMask**의 각 리스트 입력에 연결합니다.
> 7. **Filter.ByBooleanMask**의 출력은 _"in"_ 및 _"out"_ 으로 표시됩니다. _"In"_ 은 마스크 값이 _"true"_ 인 값을 나타내고 _"out"_ 은 마스크 값이 _"false"_ 인 값을 나타냅니다. **Line.ByStartPointEndPoint** 노드의 _"in"_ 출력을 _startPoint_ 및 _endPoint_ 입력에 연결하여 필터링된 선 리스트를 작성했습니다.
> 8. **Watch3D** 노드에서는 점보다 선 수가 더 적은 것을 나타냅니다. true 값만 필터링하여 노드의 25%만 선택했습니다.
