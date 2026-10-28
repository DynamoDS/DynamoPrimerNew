# Autodesk Assistant가 수행할 수 없는 작업

### 패키지 업데이트하기

Autodesk Assistant는 아직 Dynamo에 패키지를 다운로드하고 설치할 수 없습니다.

![](../../.gitbook/assets/Aa_Update.jpg)

위 이미지에서 볼 수 있듯이, Autodesk Assistant는 Dynamo와 관련된 외부 패키지를 수정하거나 설치 또는 제거할 수 없습니다. 사용자가 직접 패키지를 설치할 수 있도록 안내는 제공할 수 있습니다.


### 파일 직접 읽기 및 쓰기

Autodesk Assistant는 Dynamo 그래프 파일을 처리할 수 있습니다. 그래프를 열거나 캔버스에 삽입하고, 그래프를 실행하고, 작업 내용을 저장할 수 있습니다. 하지만 자체적으로 하드 디스크에 있는 다른 파일을 읽거나 쓸 수는 없습니다.

아래 예시에서는 캔버스에 일부 형상이 생성되어 있습니다. Autodesk Assistant에 해당 형상을 구성하는 점들을 추출하여 JSON 파일로 내보내도록 요청합니다. Autodesk Assistant는 JSON 파일을 직접 생성할 수는 없지만, 이를 수행하는 Dynamo 스크립트를 작성할 수 있다고 제안합니다.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

이러한 제안 방식을 기억해 두십시오. 그래프 외부의 모든 작업은 Assistant 자체가 아니라 노드를 통해 수행됩니다.

### 그래프 없이 Revit 모델 변경

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Dynamo용 Autodesk Assistant는 Revit 모델에 직접 연결되어 있지 않습니다. Revit용 Assistant에 요청하듯 모델을 편집해 달라고 요청하면, 편집할 수 없다고 안내합니다.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

하지만 Revit 모델을 변경할 수는 있습니다. 사용자가 직접 작업하는 방식과 마찬가지로 Revit 노드를 배치하고 서로 연결하며, 그래프를 실행하면 모델이 변경됩니다.

![](../../.gitbook/assets/Aa_viewScript.jpg)

이러한 작업 방식에는 다음 두 가지 특징이 있습니다.

- **그래프를 실행하기 전에는 아무것도 변경되지 않습니다.** 먼저 그래프를 확인하고 값을 조정할 수 있습니다. 사용자가 직접 그래프를 실행하거나 요청의 일부로 Assistant가 그래프를 실행할 때만 모델이 변경됩니다.
- **동일한 변경 작업을 반복할 수 있습니다.** 그래프는 캔버스에 그대로 유지됩니다. 그래프를 저장하면 다른 모델에서 동일한 작업을 다시 실행하거나 팀원과 공유할 수 있습니다.

그래프를 사용하지 않고 모델을 한 번만 직접 편집하려면 Revit에서 Autodesk Assistant를 사용하세요.
