# 라이브러리에 게시

앞서 사용자 노드를 작성하여 Dynamo 그래프의 특정 프로세스에 적용해 보았습니다. 이 노드가 만족스러우므로 이제 다른 그래프에서 참조할 수 있도록 Dynamo 라이브러리에 보존하려고 합니다. 이렇게 하기 위해 노드를 로컬로 게시하겠습니다. 이 과정은 패키지를 게시하는 것과 유사한 프로세스인데, 이는 다음 장에서 자세히 살펴보겠습니다.

노드를 로컬로 게시하면 새 세션을 열 때 Dynamo 라이브러리에서 해당 노드에 액세스할 수 있게 됩니다. 노드를 게시하지 않은 경우 사용자 노드를 참조하는 Dynamo 그래프의 해당 폴더에도 해당 사용자 노드가 있어야 합니다(또는 _파일>라이브러리 가져오기_를 사용하여 사용자 노드를 Dynamo로 가져와야 함).

{% hint style="warning" %}
사용자 노드 게시는 Dynamo for Revit 및 Dynamo for Civil 3d에서만 사용할 수 있습니다. Dynamo Sandbox에는 게시 기능이 없습니다.
{% endhint %}

## 연습: 사용자 노드를 로컬로 게시

> 아래 링크를 클릭하여 예제 파일을 다운로드하십시오.
>
> 전체 예시 파일 리스트는 부록에서 확인할 수 있습니다.

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

이전 섹션에서 작성한 사용자 노드로 계속 진행해 보겠습니다. PointsToSurface 사용자 노드를 열면 Dynamo 사용자 노드 편집기에 그래프가 표시됩니다. Dynamo 그래프 편집기에서 사용자 노드를 두 번 클릭하여 열 수도 있습니다.

![](<../images/6-1/3/publish custom node locally 01.jpg>)

사용자 노드를 로컬로 게시하려면 캔버스를 마우스 오른쪽 버튼으로 클릭하고 _"이 사용자 노드 게시..."_를 선택합니다.

![](<../images/6-1/3/publish custom node exercise - 02.jpg>)

위의 이미지와 유사하게 관련 정보를 채우고 _"로컬로 게시"_. 그룹 필드는 Dynamo 메뉴에서 액세스할 수 있는 주 요소를 정의합니다.

![](<../images/6-1/3/publish custom node exercise - 03.jpg>)

로컬로 게시하려는 모든 사용자 노드를 보관할 폴더를 선택합니다. 로드할 때마다 Dynamo에서 이 폴더를 확인하므로 폴더가 영구적인 위치에 있어야 합니다. 이 폴더로 이동한 후 _"폴더 선택"_을 선택합니다. Dynamo 노드가 이제 로컬로 게시되므로 프로그램을 로드할 때마다 Dynamo 도구막대에 표시됩니다.

![](<../images/6-1/3/publish custom node exercise - 04.jpg>)

사용자 노드 폴더 위치를 확인하려면 _Dynamo > 기본 설정 > Package Manager > 노드 및 패키지 경로_로 이동합니다.

![](<../images/6-1/3/publish custom node exercise - 05.jpg>)

이 창에는 경로 리스트가 표시됩니다.

![](<../images/6-1/3/publish custom node exercise - 06.jpg>)

> 1. _Documents₩DynamoCustomNodes..._는 로컬로 게시한 사용자 노드의 위치를 나타냅니다.
> 2. _AppData\Roaming\Dynamo..._는 온라인으로 설치된 Dynamo 패키지의 기본 위치를 나타냅니다.
> 3. 폴더 경로를 선택하고 경로 이름 왼쪽에 있는 아래쪽 화살표를 클릭하여 리스트 순서에서 위에 있는 로컬 폴더 경로를 아래로 이동할 수도 있습니다. 맨 위 폴더는 패키지 설치의 기본 경로입니다. 따라서 기본 Dynamo 패키지 설치 경로를 기본 폴더로 유지하면 온라인 패키지가 로컬로 게시된 노드와 분리됩니다.

Dynamo의 기본 경로를 패키지 설치 위치로 설정하기 위해 경로 이름의 순서를 전환했습니다.

![](<../images/6-1/3/publish custom node exercise - 07.jpg>)

이 로컬 폴더로 이동하면 원래 사용자 노드를 Dynamo 사용자 노드 파일의 확장자인 _".dyf"_ 폴더에서 찾을 수 있습니다. 이 폴더의 파일을 편집할 수 있으며 그러면 노드가 UI에서 업데이트됩니다. 또한 주 _DynamoCustomNode_ 폴더에 노드를 더 추가할 수도 있는데, 그러면 Dynamo가 다시 시작할 때 해당 노드를 라이브러리에 추가합니다.

![](<../images/6-1/3/publish custom node exercise - 08.jpg>)

이제 Dynamo가 매번 Dynamo 라이브러리의 "DynamoPrimer" 그룹에 있는 "PointsToSurface"와 함께 로드됩니다.

![](<../images/6-1/3/publish custom node exercise - 09.jpg>)
