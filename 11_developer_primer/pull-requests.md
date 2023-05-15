# 끌어오기 요청

Dynamo는 커뮤니티의 창의성과 헌신에 의존하고 있으며, Dynamo 팀은 기여자들이 가능성을 탐색하고, 아이디어를 테스트하고, 커뮤니티에 피드백을 제공하도록 독려합니다. 혁신은 적극 권장되지만, 변경 사항은 Dynamo의 사용 편의성을 높이고 이 문서에 정의된 지침을 충족하는 경우에만 병합됩니다. 이점이 적은 변경 사항은 병합되지 않습니다.

#### 끌어오기 요청에 대한 기대 사항 <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Dynamo 팀은 끌어오기 요청이 다음과 같은 몇 가지 지침을 준수하기를 기대합니다.

* [코딩 표준](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) 및 [노드 명명 표준을 준수합니다.](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* 새 기능을 추가할 때 단위 테스트를 포함합니다.
* 버그를 수정할 때 현재 동작이 작동하지 않는 방식을 강조 표시하는 단위 테스트를 추가합니다
* 한 가지 이슈를 집중적으로 논의합니다. 새 주제 또는 관련 주제가 발생하면 새 이슈를 생성합니다.

다음은 수행하지 말아야 하는 행동에 대한 몇 가지 지침입니다.

* 갑자기 대규모 끌어오기 요청을 진행하지 않습니다. 대신, 이슈를 제기하고 논의를 시작하여 많은 시간을 투자하기 전에 방향에 대해 서로가 합의할 수 있도록 합니다.
* 작성하지 않은 코드를 커밋하지 않습니다. Dynamo에 추가하기에 적합하다고 생각되는 코드를 발견하면 계속 진행하기 전에 이슈를 제기하고 논의를 시작합니다.
* 라이센스 관련 파일 또는 헤더를 변경하는 PR을 제출하지 않습니다. PR에 문제가 있다고 생각되면 이슈를 제출해 주십시오. 그러면 주의를 기울여 검토하겠습니다.
* 이슈를 제기하고 먼저 Autodesk와 논의를 하지 않은 상태에서 API를 추가하지 않습니다.

#### 끌어오기 요청 템플릿 작성하기 <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

끌어오기 요청을 제출할 때는 [기본 PR 템플릿](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md)을 사용하십시오. PR을 제출하기 전에, 다음과 같이 목적에 대한 설명이 명확하고 모든 선언이 사실인지 확인하십시오.

* 이 PR 이후 코드베이스의 상태가 더 개선되었습니다.
* [표준](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)에 따라 문서화되었습니다.
* 이 PR에 포함된 테스트 수준이 적절합니다.
* 사용자 대상 문자열(있는 경우)이 `*.resx` 파일로 추출됩니다.
* 모든 테스트는 셀프 서비스 CI를 사용하여 통과됩니다.
* UI 변경에 대한 스냅샷(있는 경우)
* API에 대한 변경 사항은 [유의적 버전 ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions)을 따르며 [API 변경 사항](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) 문서에 설명되어 있습니다.

끌어오기 요청에 대한 적절한 검토자는 Dynamo 팀에서 배정합니다.

#### 끌어오기 요청 검토 프로세스 <a href="#pull-request-review-process" id="pull-request-review-process"></a>

끌어오기 요청을 제출하고 나면 검토 프로세스에 계속 참여해야 할 수 있습니다. 다음 검토 기준을 숙지하십시오.

* Dynamo 팀은 한 달에 한 번 모여서 가장 오래된 끌어오기 요청부터 최신 끌어오기 요청까지 검토합니다.
* 검토된 끌어오기 요청에 소유자의 변경이 필요한 경우 PR 소유자는 30일 이내에 응답해야 합니다. 다음 세션까지 PR에 대한 활동이 진행되지 않으면 팀에서 종료하거나 유용성에 따라 팀원이 넘겨받습니다.
* 끌어오기 요청은 Dynamo의 기본 PR 템플릿을 사용해야 합니다.
* 모든 선언이 충족되었지만 Dynamo PR 템플릿이 완전히 작성되지 않은 끌어오기 요청은 검토되지 않습니다.

#### Dynamo Revit 커밋 cherry-pick하기 <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Revit의 여러 버전이 출시되어 있으므로 여러 버전의 Revit에서 새 기능을 사용할 수 있도록 특정 DynamoRevit 릴리즈 분기에 변경 사항을 cherry-pick해야 할 수 있습니다. 검토 프로세스에서 기여자는 Dynamo 팀이 지정한 대로 다른 DynamoRevit 분기에 검토된 커밋을 cherry-pick할 책임이 있습니다.
