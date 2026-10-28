# RevitMCP와 DynamoMCP

Autodesk Assistant는 MCP 서버를 통해 기능을 제공받습니다. 각 서버는 Assistant에 일련의 도구를 제공하며, 각 도구는 해당 제품에서 하나의 작업을 수행합니다.

Revit에서 작업할 때는 두 서버가 중요합니다. 바로 **RevitMCP**와 **DynamoMCP**입니다. 두 서버는 서로 경쟁하는 선택지가 아니며, 둘 중 하나를 선택할 필요도 없습니다. 각 서버는 서로 다른 수준에서 작동하며, Assistant는 요청에 적합한 서버를 사용합니다.

### RevitMCP: 모델 직접 조작

RevitMCP는 [Revit Public MCP Server](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm)를 통해 제공되며, 선별된 Revit API 작업을 MCP 도구로 래핑합니다. 각 도구는 모델에서 요소 찾기, 매개변수 및 개수 확인, 여러 매개변수 일괄 편집, 뷰 스냅샷 캡처 등 정의된 작업을 수행합니다.

이 도구 세트에서 이미 지원하는 작업이라면 가장 빠르게 처리할 수 있는 방법입니다. 요청 하나로 도구 호출 한 번이 이루어지며, 모델이 즉시 변경됩니다. 그래프를 작성하거나 별도로 실행할 필요가 없습니다.

### DynamoMCP: 작업을 구성하는 도구

DynamoMCP는 한 단계 상위 수준에서 작동합니다.. DynamoMCP의 도구는 모델링 작업을 직접 수행하지 않습니다. 대신 Dynamo 그래프를 작성합니다. 즉, 노드를 배치하고 서로 연결하며, 값을 설정하고, 완성된 그래프를 실행합니다. 전체 도구 리스트는 [Dynamo MCP 서버란 무엇입니까?](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md)를 참조하십시오.

여기서 중요한 것은 노드의 특성입니다. Dynamo 라이브러리의 각 노드는 그 자체로 하나 이상의 API 호출을 수행하며, 해당 호출은 이미 작성, 테스트 및 문서화되어 있습니다. 패키지 노드와 Python 노드도 같은 방식으로 사용할 수 있습니다. 따라서 소수의 그래프 작성 도구만으로도 Dynamo 라이브러리 전체의 기능을 활용할 수 있습니다.

그 결과, DynamoMCP의 도구는 기존 기능을 사용하는 데 그치지 않고 새로운 기능을 구성해 냅니다. 정해진 작업 리스트에서 선택하는 대신, Assistant는 노드를 조합하여 사용자가 요청한 작업을 구성합니다. 실제로 그래프를 작성하는 MCP 도구는 다른 도구를 만들어 내는 도구처럼 작동합니다.

### 실제 작업에서의 차이

한 층의 문에 태그를 추가해 달라고 요청한다고 가정해 보겠습니다.

- RevitMCP로 처리하는 경우 작업을 직접 호출하면 태그가 표시됩니다.
- \- DynamoMCP로 처리하는 경우 Assistant가 문을 수집하고, 지정한 층의 문만 필터링한 다음 태그를 생성하는 노드를 배치하고 그래프를 실행합니다.

두 방식 모두 문에 태그를 추가합니다. 두 번째 방식은 여기에 더해 캔버스에 그래프도 남깁니다.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| 도구의 기능 | 정의된 Revit 작업 수행 | Dynamo 그래프 작성 및 실행 |
| 범위 | 도구 세트에 포함된 작업 | 패키지 노드와 Python 노드를 포함한 라이브러리의 모든 노드 |
| 결과 | 모델의 변경 사항 | 모델 변경 사항과 해당 변경을 수행한 그래프 |
| 적합한 작업 | 명확하게 정의된 단일 작업 | 검사, 조정, 재사용 또는 공유하려는 로직이 포함된 작업 |

### 그래프를 만들어 두면 좋은 이유

그래프는 작업 결과뿐 아니라 작업 과정까지 기록합니다. 그래프를 캔버스에 배치하면 다음과 같은 작업을 수행할 수 있습니다.

- 그래프를 확인하여 어떤 작업이 수행되었는지 정확히 파악할 수 있습니다.
- 값을 변경한 후 전체 작업을 다시 요청하지 않고 그래프를 재실행할 수 있습니다.
- 그래프를 저장해 다음 프로젝트에서 실행하거나 동료에게 전달할 수 있습니다.
- 그래프를 Dynamo Player에 게시하여 Dynamo를 사용하지 않는 사람도 그래프를 실행할 수 있도록 할 수 있습니다.

처음 요청할 때는 시간이 들지만, 그 이후에는 거의 추가 비용 없이 매번 실행할 수 있습니다.

{% hint style="info" %}
Dynamo Sandbox에서는 작업 대상인 Revit 모델이 없으므로 Assistant가 DynamoMCP만 사용합니다. Revit에서는 두 서버를 모두 사용할 수 있습니다.
{% endhint %}

### 요청할 직업 선택하기

프롬프트에 서버 이름을 명시할 필요는 없습니다. 원하는 결과를 설명하는 것만으로 충분하며, Assistant가 적절한 도구를 선택합니다.

특정 그래프를 원한다면 그렇게 요청하면 됩니다. 예를 들어 *"2층의 문에 태그를 추가하는 그래프를 만들어 줘"*라고 요청하면, Assistant는 단순히 문에 태그를 추가하는 것뿐 아니라 재사용할 수 있는 그래프를 만드는 것이 목적임을 파악합니다.

### 추가 참고 자료

- [Autodesk Assistant(기술 미리보기)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) — Revit 2027 도움말에서 Revit용 Assistant의 기능을 설명합니다.
- [Revit Public MCP Server(기술 미리보기)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) — Revit 2027 도움말에서 MCP의 Revit 측 기능을 설명합니다.
- [Autodesk Assistant: 개요 및 자주 묻는 질문과 답변](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) — Assistant가 포함된 제품과 사용 방법을 안내합니다.
