# Dynamo 명령행 인터페이스

`-o, -O, --OpenFilePath` Dynamo에 명령 파일을 열고 이 파일에 포함된 명령을 이 경로에서 실행하도록 지시합니다. 이 옵션은 DynamoSandbox에서 실행하는 경우에만 지원됩니다.  

`-c, -C, --CommandFilePath` Dynamo에 명령 파일을 열고 이 파일에 포함된 명령을 이 경로에서 실행하도록 지시합니다. 이 옵션은 DynamoSandbox에서 실행하는 경우에만 지원됩니다.  

`-v, -V, --Verbose` Dynamo에 수행하는 모든 평가를 지정된 경로에 XML 파일로 출력하도록 지시합니다.  

`-g, -G, --Geometry` Dynamo에 모든 평가에서 생성된 형상을 이 경로에 JSON 파일로 출력하도록 지시합니다.  

`-h, -H, --help` 도움말을 표시합니다.  

`-i, -I, --Import` Dynamo에 어셈블리를 노드 라이브러리로 가져오도록 지시합니다. 이 인수는 단일 `.dll`에 대한 파일 경로여야 합니다. 여러 `.dlls`를 가져오려면 공백으로 구분하여 나열합니다(예: `-i 'assembly1.dll' 'assembly2.dll'`).  

`--GeometryPath` ASM을 포함하는 디렉토리에 대한 상대 또는 절대 경로입니다. 경고가 제공되면 하드 디스크에서 ASM을 검색하는 대신 이 경로에서 직접 로드됩니다.  

`-k, -K, --KeepAlive` 연결 유지 모드에서는 로드된 확장 프로그램이 Dynamo 프로세스를 종료할 때까지 이 프로세스를 계속 실행 상태로 둡니다.  

`--HostName` 호스트와 연관된 Dynamo 변형을 식별합니다.  

`-s, -S, --SessionId` Dynamo 호스트 분석 세션 ID를 식별합니다.  

`-p, -P, --ParentId` Dynamo 호스트 분석 상위 ID를 식별합니다.  

`-x, -X, --ConvertFile` `-O` 플래그와 함께 사용할 경우 지정된 경로에서 `.dyn` 파일을 열고 `.json`으로 변환합니다. 파일의 확장자는 `.json`이며 원본 파일과 동일한 디렉토리에 배치됩니다.  

`-n, -N, --NoConsole` 연결 유지 모드에서는 콘솔 창을 통해 CLI와 상호 작용하지 마십시오.  

`-u, -U, --UserData` CLI에서 PathResolver가 사용할 사용자 데이터 폴더를 지정합니다.  

`--CommonData` CLI에서 PathResolver가 사용할 공통 데이터 폴더를 지정합니다.  

`--DisableAnalytics` Dynamo에서 프로세스 수명 동안 분석을 비활성화합니다.  

`--CERLocation` 디스크에 있는 충돌 오류 보고서 도구를 지정합니다.  

`--ServiceMode` 서비스 모드 시작을 지정합니다.  



#### 사용하는 이유 
 다음과 같은 다양한 이유로 명령행에서 Dynamo를 제어해야 할 수 있습니다. 
 
 * 많은 Dynamo 실행 자동화
 * Dynamo 그래프 테스트(DynamoSandbox를 사용하는 경우 -c 참조)
 * 특정 순서로 일련의 Dynamo 그래프 실행
 * 여러 명령행을 실행하는 배치 파일 작성
 * Dynamo 그래프의 실행을 제어 및 자동화하고 해당 계산 결과를 사용하여 다양한 작업을 수행하는 또 다른 프로그램 작성

#### Dynamo 명령행 인터페이스란?
DynamoSandbox를 보완하는 명령행 인터페이스(DynamoCLI)는 Dynamo 실행을 위해 명령행 인수를 손쉽게 사용할 수 있도록 설계된 DOS/터미널 명령행 유틸리티입니다. 처음 구현 시 독립적으로 실행되지 않으며 Sandbox와 동일한 코어 DLL에 종속되므로 Dynamo 바이너리가 있는 폴더에서 실행해야 합니다. 다른 Dynamo 빌드와 상호 운용할 수 없습니다.

CLI는 다음과 같은 4가지 방법으로 실행할 수 있습니다. 처음 두 가지 방법은 Dos 프롬프트에서 실행하거나 Dos 배치 파일에서 실행하는 것입니다. 세 번째 방법은 Windows 데스크톱 바로 가기로 실행하는 것이며, 바로 가기 경로는 지정된 명령행 플래그를 포함하도록 수정되었습니다. DOS 파일 사양은 정규화되거나 상대적일 수 있으며 매핑된 드라이브 및 URL 구문도 지원됩니다. 네 번째 방법으로, Mono를 사용하여 빌드한 후 Linux 또는 Mac에서 터미널을 통해 실행할 수도 있습니다.

이 유틸리티에서는 Dynamo 패키지를 지원하지만 사용자 노드(dyf)는 로드할 수 없으며 독립 실행형 그래프(dyn)만 로드할 수 있습니다.

예비 테스트에서 CLI 유틸리티는 현지화된 버전의 Windows를 지원하며 대문자 ASCII 문자로 filespec 인수를 지정할 수 있습니다.

CLI에는 DynamoCLI.exe 응용프로그램을 통해 액세스할 수 있습니다. 이 응용프로그램을 사용하면 사용자 또는 다른 응용프로그램이 명령 문자열을 통해 DynamoCLI.exe를 호출하여 Dynamo 평가 모델과 상호 작용할 수 있습니다. 상호 작용하는 방식은 다음과 같을 수 있습니다.
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
이 명령은 UI를 그리지 않고 *"C:\\someReallyCoolDynamoFile.Dyn"* 에서 지정된 파일을 연 다음 실행하도록 Dynamo에 지시합니다. 그런 다음 그래프 실행이 완료되면 Dynamo가 종료됩니다. 

**버전 2.1의 새로운 기능**: DynamoWPFCLI.exe 응용프로그램. 이 응용프로그램은 형상(-g) 옵션 이외에 DynamoCLI.exe 응용프로그램이 지원하는 모든 것을 지원합니다. DynamoWPFCLI.exe 응용프로그램은 Windows에서만 사용할 수 있습니다.

#### 중요 참고 사항

* DynamoCLI와 상호 작용하는 기본 방법은 명령 프롬프트 인터페이스를 사용하는 것입니다.
* 이 경우 [Dynamo 버전] 폴더 내의 설치 위치에서 DynamoCLI를 실행해야 합니다. CLI는 Dynamo와 동일한 .dll에 액세스해야 하므로 이동해서는 안 됩니다.
* 현재 Dynamo에 열려 있는 그래프를 실행할 수 있어야 하지만 이로 인해 의도치 않은 결과가 발생할 수 있습니다.
* 모든 파일 경로는 DOS와 완벽하게 호환되므로 상대 경로와 정규화된 경로도 작동하지만 *"C:path\\to\\file.dyn"* 처럼 경로를 따옴표로 묶어야 합니다. 
* DynamoCLI는 새로운 기능이며 현재 개발 중입니다. 현재 **CLI는 표준 Dynamo 라이브러리의 하위 세트만 로드** 합니다. 그래프가 제대로 실행되지 않는 경우 이 점에 유의하십시오. 이러한 라이브러리는 [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28)에서 지정됩니다. 
* 현재 std 출력은 제공되지 않습니다. 오류가 발생하지 않으면 실행이 완료된 후 CLI가 종료됩니다.
 
#### 사용 방법

`-o`: 그래프를 실행하는 헤드리스 모드에서 .dyn을 지정하여 Dynamo를 열 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v`: (`-o` 플래그를 사용하여 파일을 연 경우) Dynamo를 헤드리스 모드에서 실행 중이면 사용할 수 있습니다. 이 플래그는 그래프의 모든 노드를 반복하고 해당 출력 값을 간단한 XML 파일에 덤프합니다. `--ServiceMode` 플래그는 Dynamo가 여러 그래프 평가를 강제로 실행하도록 할 수 있으므로 출력 파일에는 수행되는 각 평가에 대한 값이 저장됩니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
XML 출력 파일의 형식은 다음과 같습니다.
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g`: (`-o` 플래그를 사용하여 파일을 연 경우) Dynamo를 헤드리스 모드에서 실행 중이면 사용할 수 있습니다. 이 플래그는 그래프를 생성한 다음 결과 형상을 JSON 파일로 덤프합니다. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
JSON 형상 파일의 형식은 다음과 같습니다.

 TBD - 작업 진행 중\`

`-h`: 가능한 옵션 리스트를 가져오려면 사용합니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

-i 플래그는 열려는 그래프를 실행하는 데 필요한 여러 개의 어셈블리를 가져오기 위해 여러 번 사용할 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

-l 플래그는 다른 로케일 설정에서 Dynamo를 실행하는 데 사용할 수 있습니다. 그러나 일반적으로 로케일 설정은 그래프 결과에 영향을 주지 않습니다

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

--GeometryPath 플래그는 DynamoSandbox 또는 CLI가 특정 ASM 바이너리 세트를 지정하도록 사용할 수 있습니다. 사용 예시는 다음과 같습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

또는

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

-k 플래그는 로드된 확장이 Dynamo 프로세스를 종료할 때까지 이 프로세스가 계속 실행되도록 하는 데 사용할 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

--HostName 플래그는 호스트와 연관된 Dynamo 변형을 식별하는 데 사용할 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

또는

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

-s 플래그는 Dynamo 호스트 분석 세션 ID를 식별하는 데 사용할 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

-p 플래그는 Dynamo 호스트 분석 상위 ID를 식별하는 데 사용할 수 있습니다.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
