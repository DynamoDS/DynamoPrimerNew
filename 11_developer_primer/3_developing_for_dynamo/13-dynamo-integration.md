# Integração do Dynamo

Esta é a documentação de integração para a linguagem de programação visual do Dynamo.

Este guia discutirá vários aspectos sobre como hospedar o Dynamo no aplicativo para permitir que os usuários interajam com o aplicativo usando a programação visual.

Conteúdo:

* [Esta introdução](13-dynamo-integration.md#dynamo-integration): visão geral de alto nível do que este guia inclui e o que é o Dynamo.
* [Ponto de entrada personalizado do Dynamo](13-dynamo-integration.md#dynamo-custom-entry-point): como criar um DynamoModel e por onde começar.
* [Vinculação e rastreamento de elementos](13-dynamo-integration.md#-element-binding-and-trace): usar o mecanismo de rastreamento do Dynamo para vincular os nós no gráfico a seus resultados no hospedeiro.
* [Nós de seleção do Revit do Dynamo](13-dynamo-integration.md#-dynamo-revit-selection-nodes): como implementar nós que permitem aos usuários selecionar objetos ou dados do hospedeiro e passá-los como entradas para o gráfico do Dynamo.
* [Visão geral dos pacotes integrados do Dynamo](13-dynamo-integration.md#dynamo-built-in-packages-overview): o que é a Biblioteca padrão do Dynamo e como usar o mecanismo subjacente para enviar pacotes com a integração.

**Um pouco de dicção:**

Usaremos os termos script, gráfico e programa do Dynamo de forma intercambiável nestes documentos para nos referirmos ao código que os usuários criam no Dynamo.

## Ponto de entrada personalizado do Dynamo

#### Revit do Dynamo como exemplo

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

O `DynamoModel` é o ponto de entrada para um aplicativo que hospeda o Dynamo. Ele representa um aplicativo do Dynamo. O modelo é o objeto raiz de nível superior que contém referências a outras estruturas de dados e objetos importantes que compõem o aplicativo Dynamo e a máquina virtual do DesignScript.

Um objeto de configuração é usado para definir parâmetros comuns no `DynamoModel` quando ele é construído.

Os exemplos neste documento foram retirados da implementação do DynamoRevit, que é uma integração na qual o Revit hospeda um `DynamoModel` como um complemento (arquitetura de plug-in para Revit). Quando esse complemento é carregado, ele inicia um `DynamoModel` e, em seguida, o exibe para o usuário com um `DynamoView` e `DynamoViewModel`.

O Dynamo é um projeto .net em c#. Para usá-lo em seu aplicativo, você precisa ser capaz de hospedar e executar código .net.

O DynamoCore é um mecanismo de computação de plataforma cruzada e uma coleção de modelos principais, que podem ser criados com .net ou mono (no futuro .net core). Mas o DynamoCoreWPF contém os componentes de interface do usuário do Dynamo somente para Windows e não será compilado em outras plataformas.

### Etapas para personalizar o ponto de entrada do Dynamo

Para inicializar o `DynamoModel`, os integradores precisarão executar estas etapas em algum lugar no código do hospedeiro.

### Pré-carregar as DLLs do Dynamo compartilhadas do hospedeiro.

No momento, a lista no D4R inclui apenas `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` Isso é feito para evitar conflitos de versões de bibliotecas entre o Dynamo e o Revit. Por exemplo: quando ocorrem conflitos em `AvalonEdit`, a função do bloco de código pode ser totalmente interrompida. O problema é reportado no Dynamo 1.1.x em [https://github.com/DynamoDS/Dynamo/issues/7130](https://github.com/DynamoDS/Dynamo/issues/7130) e também é reproduzível manualmente. Se os integradores encontrarem conflitos de bibliotecas entre a função do hospedeiro e o Dynamo, recomenda-se fazer isso como primeiro passo. Às vezes, isso é necessário para impedir que outro plug-in ou o próprio aplicativo hospedeiro carregue uma versão incompatível de uma dependência compartilhada. Uma solução melhor é resolver o conflito de versões alinhando a versão ou usar um redirecionamento de vinculação .net no app.config do hospedeiro, se possível.

### Carregar o ASM

#### O que são a ASM e o LibG

A ASM é a biblioteca de geometria ADSK com base na qual o Dynamo foi construído.

O LibG é um wrapper .Net fácil de usar em torno do kernel de geometria ASM. O libG compartilha seu esquema de versões com a biblioteca ASM; ele usa o mesmo número de versão principal e secundária da ASM para indicar que é o wrapper correspondente de uma versão específica da ASM. Quando é fornecida uma versão da ASM, a versão do libG correspondente deve ser a mesma. O LibG, na maioria dos casos, deve funcionar com todas as versões da ASM de uma determinada versão principal. Por exemplo, o LibG 223 deve ser capaz de carregar qualquer versão da ASM 223.

#### Dynamo Sandbox carregando a ASM

O Dynamo Sandbox foi desenvolvido para funcionar com diversas versões da ASM. Para isso, diversas versões do libG são agrupadas e enviadas com o núcleo. Há uma funcionalidade integrada no Dynamo Shape Manager para procurar produtos da Autodesk que são fornecidos com a ASM, para que o Dynamo possa carregar a ASM desses produtos e fazer com que os nós de geometria funcionem sem serem carregados explicitamente em um aplicativo hospedeiro. A lista de produtos existente hoje é:

```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```

O Dynamo pesquisará no registro do Windows e descobrirá se os produtos da Autodesk nessa lista estão instalados no computador do usuário. Se algum deles estiver instalado, ele pesquisará por binários da ASM, obterá a versão e procurará uma versão do libG correspondente no Dynamo.

Dada a versão da ASM, a seguinte API do ShapeManager escolherá a localização do pré-carregador libG correspondente para carregar. Se houver uma correspondência exata da versão, ela será usada; caso contrário, o libG com a versão mais próxima abaixo, mas com a mesma versão principal, será carregada.

Por exemplo: se o Dynamo estiver integrado com uma compilação de desenvolvimento do Revit, em que há uma compilação da ASM 225.3.0 mais recente, o Dynamo tentará usar o libG 225.3.0, se existir; caso contrário, ele tentará usar a versão principal mais próxima, menor que sua primeira escolha, ou seja, 225.0.0.

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)`

#### Integração em processo do Dynamo carregando a ASM do hospedeiro

O Revit é a primeira entrada na lista de pesquisa de produtos da ASM, o que significa que, por padrão, o `DynamoSandbox.exe` tentará carregar a ASM do Revit primeiro. Ainda queremos garantir que a sessão de trabalho integrada do D4R carregue a ASM do hospedeiro atual do Revit: por exemplo, se o usuário tiver o R2018 e o R2020 no computador, ao iniciar o D4R do R2020, o D4R deverá usar a ASM 225 do R2020 em vez da ASM 223 do R2018. Os integradores precisarão implementar chamadas similares às seguintes para forçar o carregamento de sua versão especificada.

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### O Dynamo carregando a ASM de um caminho personalizado

Recentemente, adicionamos a capacidade de o `DynamoSandbox.exe` e o `DynamoCLI.exe` carregarem uma versão da ASM específica. Para ignorar o comportamento normal de pesquisa do registro, é possível usar o indicador `--GeometryPath` para forçar o Dynamo a carregar a ASM de um caminho específico.

`DynamoSandbox.exe --GeometryPath "somePath/To/ASMDirectory"`

### Criar um StartConfiguration

O StartupConfiguration é usado para ser passado como um parâmetro para inicializar o DynamoModel, o que indica que ele contém quase todas as definições de como você gostaria de personalizar suas configurações de sessão do Dynamo. Dependendo de como as propriedades a seguir são definidas, a integração do Dynamo pode variar entre diferentes integradores. Por exemplo, diferentes integradores podem definir diferentes caminhos de modelo Python ou formatos de números exibidos.

Consiste no seguinte:

* DynamoCorePath // Onde os binários de carregamento do DynamoCore estão localizados
* DynamoHostPath // Onde os binários de integração do Dynamo estão localizados
* GeometryFactoryPath // Onde os binários libG carregados estão localizados
* PathResolver // Objeto que ajuda a resolver vários arquivos
* PreloadLibraryPaths // Onde estão localizados os nós binários pré-carregados, por exemplo, DSOffice.dll
* AdditionalNodeDirectories // Onde os binários dos nós adicionais estão localizados
* AdditionalResolutionPaths // Caminhos de resolução de montagem adicionais para outras dependências que podem ser necessárias durante o carregamento de bibliotecas
* UserDataRootFolder // Pasta de dados do usuário, por exemplo, `"AppData\Roaming\Dynamo\Dynamo Revit"`
* CommonDataRootFolder // Pasta padrão para salvar definições personalizadas, amostras etc.
* Context // Nome do hospedeiro do integrador + versão `(Revit<BuildNum>)`
* SchedulerThread // Thread do programador do integrador implementando o `ISchedulerThread`. Para a maioria dos integradores, esse é o thread da interface principal ou qualquer thread de onde eles podem acessar sua API.
* StartInTestMode // Se a sessão atual é uma sessão de automação de teste (modifica vários comportamentos do Dynamo), não use, a menos que esteja escrevendo testes.
* AuthProvider // Implementação do integrador do IAuthProvider, por exemplo, a implementação do RevitOxygenProvider está no Greg.dll, usado para integração de carregamento do packageManager.

### Preferências

O caminho de configuração de preferência padrão é gerenciado por `PathManager.PreferenceFilePath`, por exemplo, `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`. Os integradores podem decidir se desejam também enviar um arquivo de configuração de preferência personalizado para uma localização que precisa ser alinhada com o gerenciador de caminho. A seguir, estão as propriedades de configuração de preferência que são serializadas:

* IsFirstRun // Indica se é a primeira vez que esta versão do Dynamo é executada, por exemplo, usado para determinar se é necessário exibir a mensagem de aceitação/exclusão do GA. Também usado para determinar se é necessário migrar a configuração de preferência do Dynamo legado ao iniciar uma nova versão do Dynamo, para que os usuários tenham uma experiência consistente
* IsUsageReportingApproved // Indica se o relatório de uso foi aprovado ou não
* IsAnalyticsReportingApproved // Indica se o relatório analítico foi aprovado ou não
* LibraryWidth // A largura do painel esquerdo da biblioteca do Dynamo.
* ConsoleHeight // A altura da tela do console
* ShowPreviewBubbles // Indica se as bolhas de visualização devem ser exibidas
* ShowConnector // Indica se os conectores são exibidos
* ConnectorType // Indica o tipo de conector: Bezier ou Polilinha
* BackgroundPreviews // Indica o estado ativo da visualização do plano de fundo especificada
* RenderPrecision // O nível de precisão de renderização; menor gera malhas com menos triângulos. Maior gerará uma geometria mais suave na visualização do plano de fundo. 128 é um bom número rápido para a geometria de visualização.
* ShowEdges // Indica se as arestas de sólido e superfície serão renderizadas
* ShowDetailedLayout // NÃO USADO
* WindowX, WindowY // Última coordenada X, Y da janela do Dynamo
* WindowW, WindowH // Última largura, altura da janela do Dynamo
* UseHardwareAcceleration // O Dynamo deve usar a aceleração por hardware, se for compatível
* NumberFormat // A precisão decimal usada para exibir os números em toString() da bolha de visualização.
* MaxNumRecentFiles // O número máximo de caminhos de arquivo recentes a serem salvos
* RecentFiles // Uma lista de caminhos de arquivos abertos recentemente. Tocar aqui afetará diretamente a lista de arquivos recentes na página inicial do Dynamo
* BackupFiles // Uma lista de caminhos de arquivo de backup
* CustomPackageFolders // Uma lista de pastas contendo binários sem toque e caminhos de diretório que serão verificados em busca de pacotes e nós personalizados.
* PackageDirectoriesToUninstall // Uma lista de pacotes usados pelo Gerenciador de pacotes para determinar quais pacotes estão marcados para exclusão. Esses caminhos serão excluídos, se possível, durante a inicialização do Dynamo.
* PythonTemplateFilePath // Caminho para o arquivo Python (.py) para ser usado como modelo inicial ao criar um novo nó PythonScript. Pode ser usado para configurar um modelo Python personalizado para a integração.
* BackupInterval // Indica em quanto tempo (em milissegundos) o gráfico será salvo automaticamente
* BackupFilesCount // Indica quantos backups serão feitos
* PackageDownloadTouAccepted // Indica se o usuário aceitou os termos de uso para fazer o download dos pacotes do gerenciador de pacotes
* OpenFileInManualExecutionMode // Indica o estado padrão da caixa de seleção “Abrir no modo manual” na OpenFileDialog
* NamespacesToExcludeFromLibrary // Indica quais namespaces (se houver) não devem ser exibidos na biblioteca de nós do Dynamo. Formato da sequência de caracteres: “[nome da biblioteca]:[namespace totalmente qualificado]”

Um exemplo de configurações de preferência serializadas:

```xml
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
```

* Extensions // Uma lista de extensões que implementam a IExtension. Se for nulo, o Dynamo carregará as extensões do caminho padrão (pasta `extensions` na pasta Dynamo)
* IsHeadless // Indica se o Dynamo é iniciado sem a interface do usuário. Afeta o Analytics.
* UpdateManager // Implementação do UpdateManager do integrador. Veja a descrição acima
* ProcessMode // Equivalente a TaskProcessMode. Síncrono se estiver em modo de teste; caso contrário, Assíncrono. Isso controla o comportamento do agendador. Os ambientes de thread único também podem definir isso como síncrono.

Use o StartConfiguration de destino para iniciar o `DynamoModel`

Depois que o StartConfig for passado para iniciar o `DynamoModel`, o DynamoCore supervisionará as especificações reais para garantir que a sessão do Dynamo seja inicializada corretamente com os detalhes especificados. Deve haver algumas etapas de configuração que os integradores individuais precisarão executar após a inicialização do `DynamoModel`. Por exemplo, no D4R, os eventos são inscritos para monitorar transações do hospedeiro do Revit ou atualizações de documentos, personalização do nó Python etc.

### Vamos já à parte de “programação visual”

Para inicializar o `DynamoViewModel` e o `DynamoView`, primeiro é preciso construir um `DynamoViewModel`, o que pode ser feito usando o método estático `DynamoViewModel.Start`. Veja abaixo:

```c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

O `DynamoViewModel.StartConfiguration` tem muito menos opções do que a configuração do modelo. Esses nós são, em sua maioria, autoexplicativos. O `CommandFilePath` pode ser ignorado, a menos que você esteja escrevendo um caso de teste.

O parâmetro `Watch3DViewModel` controla como a visualização do plano de fundo e os nós watch3d exibem a geometria 3D. Será possível usar sua própria implementação se implementar as interfaces necessárias.

Para construir o `DynamoView`, basta o `DynamoViewModel`. A Vista é um controle de janela e pode ser exibida usando o WPF.

### Exemplo do DynamoSandbox.exe:

O DynamoSandbox.exe é um ambiente de desenvolvimento para testar, usar e experimentar o DynamoCore. É um ótimo exemplo para verificar como os componentes `DynamoCore` e `DynamoCoreWPF` são carregados e configurados. É possível ver alguns dos pontos de entrada [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37).

## Vinculação e rastreamento de elementos

#### Visão geral

O _rastreamento_ é um mecanismo no Dynamo Core, que é capaz de serializar dados no arquivo .dyn (arquivo do Dynamo). Fundamentalmente, esses dados são vinculados aos locais de chamada dos nós dentro do gráfico do Dynamo.

Quando um gráfico do Dynamo é aberto do disco, os dados de rastreamento salvos nele são reassociados aos nós do gráfico.

#### Glossário:

* Mecanismo de rastreamento:
  * Implementa a vinculação de elementos no Dynamo
  * É possível usar o mecanismo de rastreamento para garantir que os objetos sejam vinculados novamente à geometria que eles criaram
  * O mecanismo de rastreamento e o local de chamada fornecem um GUID persistente que o implementador de nós pode usar para revincular
* Local de chamada
  * O executável contém vários locais de chamada. Esses ocais de chamada são usados ​​para despachar a execução para os vários lugares de onde eles precisam ser despachados:
    * Biblioteca em C#
    * Método integrado
    * Função DesignScript
    * Nó personalizado (função DS)
* TraceSerializer
  * Serializa classes marcadas com `ISerializable` e `[Serializable]` no rastreamento.
  * Lida com a serialização e a desserialização dos dados no rastreamento.
  * O TraceBinder controla a vinculação de dados desserializados a um tipo de tempo de execução (cria a instância de classe real).

#### Como é?

***

Os dados de rastreamento são serializados no arquivo .dyn dentro de uma propriedade chamada Bindings. Essa é uma matriz de callsites-ids -> dados. Um local de chamada é a localização/instância específica de onde um nó é chamado na máquina virtual do DesignScript. Vale a pena mencionar que os nós em um gráfico do Dynamo podem ser chamados várias vezes e, portanto, vários locais de chamada podem ser criados para uma única instância de nó.

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

_NÃO_ é recomendável depender do formato dos dados serializados codificados em base64.

#### Que problema estamos tentando resolver.

***

Há muitos motivos pelos quais alguém gostaria de salvar dados arbitrários como resultado da execução de uma função, mas neste caso o rastreamento foi desenvolvido para resolver um problema específico que os usuários encontram com frequência ao criar e iterar em programas de software que criam elementos em aplicativos hospedeiros.

O problema é aquele que chamamos de `Element Binding` e a ideia é esta:

À medida que um usuário desenvolve e executa um gráfico do Dynamo, ele provavelmente gerará novos elementos no modelo do aplicativo hospedeiro. Para o nosso exemplo, digamos que o usuário tenha um pequeno programa que gera 100 portas em um modelo arquitetônico. O número e a localização dessas portas são controlados pelo programa.

A primeira vez que o usuário executa o programa, ele gera essas 100 portas.

Mais tarde, quando o usuário modificar uma entrada em seu programa e executá-la novamente, seu programa _(sem vinculação de elementos)_ criará 100 novas portas. As portas antigas ainda existirão no modelo junto com as novas.

***

Como o Dynamo é um ambiente de programação ativo e apresenta um modo de execução `"Automatic"`, em que as alterações no gráfico acionam uma nova execução, isso pode rapidamente sobrecarregar um modelo com os resultados de muitas execuções do programa.

Descobrimos que isso geralmente não é o que os usuários esperam. Em vez disso, com a vinculação de elementos ativada, os resultados anteriores da execução de um gráfico são limpos e excluídos ou modificados. Qual deles (_excluir ou modificar_) depende da flexibilidade da API do hospedeiro. Com a vinculação de elementos ativada, após a segunda, terceira ou 50ª execução do programa do Dynamo do usuário, haverá apenas 100 portas no modelo.

Isso requer mais do que apenas ser capaz de serializar dados no arquivo .dyn, e, como você verá abaixo, há mecanismos no DynamoRevit criados sobre o rastreamento para dar suporte a esses fluxos de trabalho de revinculação.

***

Este é o momento apropriado para mencionar o outro caso de uso importante da vinculação de elementos para hospedeiros como o Revit. Como os elementos que foram criados quando a vinculação de elementos foi ativada tentarão manter as IDs dos elementos existentes (modificar elementos existentes), a lógica que foi criada sobre esses elementos no aplicativo hospedeiro continuará existindo após a execução de um programa do Dynamo. Por exemplo:

Vamos voltar ao nosso exemplo de modelo arquitetônico.

Vamos primeiro executar um exemplo com a vinculação de elementos desativada. Desta vez, o usuário tem um programa que gera algumas paredes arquitetônicas.

Ele executa o programa, que gera algumas paredes no aplicativo hospedeiro. Em seguida, ele sai do gráfico do Dynamo e usa as ferramentas normais do Revit para colocar algumas janelas nessas paredes. As janelas são vinculadas a essas paredes específicas como parte do modelo Revit.

O usuário inicia o Dynamo novamente e executa o gráfico novamente. Agora, como no nosso último exemplo, ele tem dois conjuntos de paredes. O primeiro conjunto tem janelas adicionadas, mas as novas paredes não.

Se a vinculação de elementos tiver sido ativada, será possível manter o trabalho existente que foi feito manualmente no aplicativo hospedeiro sem o Dynamo. Por exemplo, se a vinculação estivesse ativada quando o usuário executasse o programa pela segunda vez, as paredes seriam modificadas, não excluídas, e as alterações posteriores feitas no aplicativo hospedeiro persistiriam. O modelo conteria paredes com janelas, em vez de dois conjuntos de paredes em vários estados.

***

![Criar paredes](images/creates_walls.png)

#### Vinculação de elementos em comparação com rastreamento

***

O rastreamento é um mecanismo no Dynamo Core. Ele usa uma variável estática de locais de chamada para os dados para mapear os dados para o local de chamada de uma função no gráfico, conforme descrito acima.

Ele também permite serializar dados arbitrários no arquivo .dyn ao gravar nós do Dynamo sem toque. Isso geralmente não é aconselhável, pois significa que o código sem toque potencialmente transferível agora depende do Dynamo Core.

Não confie no formato serializado dos dados no arquivo .dyn. Em vez disso, use o atributo [Serializable] e a interface

O ElementBinding, por outro lado, é construído com base nas APIs de rastreamento e é implementado na integração do Dynamo _(DynamoRevit, Dynamo4Civil etc.)_

#### APIs de rastreamento

Algumas das APIs de rastreamento de baixo nível que vale a pena conhecer são:

```c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

É possível vê-las em uso no exemplo abaixo

Para interagir com os dados de rastreamento que o Dynamo carregou de um arquivo existente ou está gerando, veja:

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```

[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)

#### Exemplo de rastreamento simples de um nó

***

Um exemplo de nó do Dynamo que usa rastreamento diretamente é fornecido aqui no [repositório do DynamoSamples](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs)

O resumo da classe explica a essência do que é o rastreamento:

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

Esse exemplo usa as APIs de rastreamento no DynamoCore diretamente para armazenar alguns dados sempre que um nó específico é executado. Nesse caso, um dicionário desempenha o papel do modelo do aplicativo hospedeiro, como o banco de dados de modelos do Revit.

A configuração bruta é:

Uma classe utilitária estática `TraceExampleWrapper` é importada como um nó no Dynamo. Ela contém um único método `ByString` que cria `TraceExampleItem`. Esses são objetos .net regulares que contêm uma propriedade `description`.

Cada `TraceExampleItem` é serializado em um rastreamento representado como um `TraceableId`. Essa é apenas uma classe que contém um `IntId` que é marcado como `[Serializeable]` para que possa ser serializado com o formatador `SOAP`. Consulte [aqui para obter mais informações sobre o atributo serializável](https://docs.microsoft.com/pt-br/dotnet/api/system.serializableattribute?view=netframework-4.8)

Também é preciso implementar a interface `ISerializable` definida [aqui](https://docs.microsoft.com/pt-br/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8)

```c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

Essa classe é criada para cada `TraceExampleItem` que desejamos salvar no rastreamento, serializada, codificada em base64 e salva no disco quando o gráfico é salvo para que as vinculações possam ser reassociadas, mesmo mais tarde, quando o gráfico for aberto novamente sobre um dicionário de elementos existente. Isso não funcionará bem no caso deste exemplo, pois o dicionário não é realmente persistente como um documento do Revit.

Finalmente, a última parte da equação é o `TraceableObjectManager`, que é semelhante ao `ElementBinder` no `DynamoRevit`. Ele gerencia o relacionamento entre os objetos presentes no modelo de documento do hospedeiro e os dados que armazenamos no rastreamento do Dynamo.

Quando um usuário executa um gráfico contendo o nó `TraceExampleWrapper.ByString` pela primeira vez, um novo `TraceableId` é criado com uma nova ID, o `TraceExampleItem` é armazenado no dicionário mapeado para essa nova ID e o `TraceableID` é armazenado no rastreamento.

Na próxima execução do gráfico, procuramos no rastreamento, encontramos a ID que armazenamos lá, encontramos o objeto mapeado para essa ID e retornamos esse objeto. Em vez de criar um objeto novo, modificamos o existente.

O fluxo de duas execuções consecutivas do gráfico que cria um único `TraceExampleItem` é assim:

![Primeira chamada](images/Trace-first-call.png)

![Segunda chamada](images/Trace-second-call.png)

A mesma ideia é ilustrada no próximo exemplo com um caso de uso de nó DynamoRevit mais realista.

#### Diagrama de rastreamento

![Etapas de rastreamento](images/trace_diagram.png) – ![Fluxo de rastreamento](images/trace_alt_diagram.png)

#### OBSERVAÇÃO:

Em versões recentes do Dynamo, o uso do TLS (Thread Local Storage) foi substituído pelo uso do membro estático.

#### Exemplo de implementação da vinculação de elementos

***

Vamos analisar rapidamente como fica um nó que usa a vinculação de elementos quando implementado no DynamoRevit. Isso é análogo ao tipo de nó usado acima nos exemplos de criação de paredes fornecidos.

***

```c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

O código acima ilustra um construtor de exemplo para um elemento de parede. Esse construtor seria chamado de um nó no Dynamo como: `Wall.byParams`

Fases importantes da execução do construtor em relação à vinculação de elementos:

1. Use o `elementBinder` para verificar se há algum objeto criado anteriormente que foi vinculado a este local de chamada em uma execução anterior. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. Em caso afirmativo, tente modificar essa parede em vez de criar uma nova.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. Caso contrário, crie uma nova parede.

```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. Exclua o elemento antigo que acabamos de recuperar do rastreamento e adicione o novo para que possamos procurá-lo no futuro:

```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### Discussão

#### Eficiência

* Atualmente, cada objeto de rastreamento serializado é serializado usando a formatação xml SOAP, que é bastante detalhada e duplica muitas informações. Em seguida, os dados são codificados em base64 duas vezes. Isso não é eficiente em termos de serialização ou desserialização. Isso poderá ser melhorado no futuro se o formato interno não for construído em cima dele. Novamente, repetimos, não confie no formato dos dados serializados, quando inativos.

#### O ElementBinding deve estar ativado por padrão?

* Há casos de uso em que a vinculação de elementos não é desejada. E se alguém for um usuário avançado do Dynamo desenvolvendo um programa que deve ser executado diversas vezes para gerar agrupamentos aleatórios de elementos? A intenção do programa é criar elementos adicionais cada vez que o programa for executado. Esse caso de uso não é facilmente alcançável sem soluções alternativas para impedir que a vinculação de elementos funcione. É possível desativar o elementBinding no nível de integração, mas provavelmente essa deve ser uma funcionalidade central do Dynamo. Não está claro qual o nível de granularidade necessário para essa funcionalidade: Nível do nó? Nível do local de chamada? Sessão inteira do Dynamo? Espaço de trabalho? etc.

## Nós de seleção do Revit do Dynamo (o que são?)

Em geral, esses nós permitem que o usuário descreva de alguma forma um subconjunto do documento ativo do Revit que deseja referenciar. Há várias formas pelas quais o usuário pode referenciar um elemento do Revit (descrito abaixo). A saída resultante do nó pode ser um wrapper de elemento do Revit (wrapper do DynamoRevit) ou alguma geometria do Dynamo (convertida da geometria do Revit). A diferença entre esses tipos de saída será útil para considerar no contexto de outras integrações de hospedeiro.

Em um nível superior, **uma boa maneira de conceituar esses nós é como uma função que aceita uma ID de elemento e que retorna um ponteiro para esse elemento ou alguma geometria que representa esse elemento.**

Há vários nós `Selection` no DynamoRevit. Podemos dividi-los em pelo menos dois grupos:

![Nós de seleção do Revit](images/revitSelectionNodes.png)

1.  Seleção da interface do usuário:

    Exemplo de nós `DynamoRevit` nesta categoria são `SelectModelElement`, `SelectElementFace`

    Esses nós permitem que o usuário alterne para o contexto da interface do usuário do Revit e selecione um elemento ou conjunto de elementos, as IDs desses elementos são capturadas e é executada alguma função de conversão: um wrapper é criado ou a geometria é extraída e convertida do elemento. A conversão executada depende do tipo de nó que o usuário escolhe.
2.  Consulta de documento:

    Os nós de exemplo nessa categoria são `AllElementsOfClass`, `AllElementsOfCategory`

    Esses nós permitem que o usuário consulte todo o documento em busca de um subconjunto de elementos. Esses nós normalmente retornam wrappers que apontam para os elementos subjacentes do Revit. Esses wrappers são essenciais para a experiência do DynamoRevit, permitindo funcionalidades mais avançadas, como vinculação de elementos, e permitindo que os integradores do Dynamo selecionem quais APIs de hospedeiro serão expostas como nós para os usuários.

### Fluxos de trabalho do usuário do Revit do Dynamo:

#### Casos de exemplo

1.
   * O usuário seleciona uma parede do Revit com `SelectModelElement`. Um wrapper de parede do Dynamo é retornado ao gráfico (visível na bolha de visualização do nó)
   * O usuário coloca o nó Element.Geometry e anexa a saída do `SelectModelElement` a esse novo nó. A geometria da parede envolvida é extraída e convertida em geometria do Dynamo usando a API libG.
   * O usuário alterna o gráfico para o modo de execução automática.
   * O usuário modifica a parede original no Revit.
   * O gráfico é executado novamente de forma automática quando o documento do Revit gera um evento sinalizando que alguns elementos foram atualizados. O nó de seleção observa esse evento e vê que a ID do elemento selecionado foi modificada.

### Fluxos de trabalho do usuário do DynamoCivil:

Os fluxos de trabalho no D4C são muito semelhantes à descrição acima para o Revit. Veja dois conjuntos típicos de nós de seleção no D4C:

![Nós de seleção do Civil 3D](images/civilSelectionNodes.png)

### Problemas:

*   Por causa do atualizador de modificação de documentos que os nós de seleção no `DynamoRevit` implementam, os loops infinitos são fáceis de construir: imagine um nó monitorando o documento para todos os elementos e, em seguida, criando novos elementos em algum lugar a jusante desse nó. Este programa, quando executado, ativará um loop. O `DynamoRevit` tenta capturar esses casos de várias formas usando IDs de transação e, para isso, evita modificar o documento quando as entradas para os construtores de elementos não foram alteradas.

    Isso precisará ser considerado se a execução automática do gráfico for iniciada quando um elemento selecionado for modificado no aplicativo hospedeiro.
* Os nós de seleção no `DynamoRevit` são implementados no projeto `RevitUINodes.dll` que faz referência ao WPF. Isso pode não ser um problema, mas vale a pena estar ciente disso, dependendo da plataforma de destino.

### Diagramas de fluxo de dados

![Fluxo de seleção](images/selectModelElement.png)

![Fluxo de seleção2](images/selectElementFace.png)

### Implementação técnica: (consulte os diagramas acima):

Os nós de seleção são implementados herdando os tipos de `SelectionBase` genéricos: `SelectionBase<TSelection, TResult>` e um conjunto mínimo de membros:

* Implementação de um método `BuildOutputAST`. Esse método precisa retornar um AST, que será executado em algum momento no futuro, quando o nó for executado. No caso de nós de seleção, ele deve retornar elementos ou geometria das IDs de elementos. [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280)
* A implementação do `BuildOutputAST` é uma das partes mais difíceis da implementação de nós `NodeModel`/da interface do usuário. É melhor colocar o máximo de lógica possível em uma função c# e simplesmente incorporar um nó de chamada de função AST no AST. Observe que aqui, o `node` é um nó AST na árvore de sintaxe abstrata, não um nó no gráfico do Dynamo.

![Fluxo de seleção2](images/selectionAST.png)

* Serialização
  *   Como esses são tipos derivados explícitos do `NodeModel` (não do ZeroTouch), eles também exigem a implementação de um [JsonConstructor] que será usado durante a desserialização do nó em um arquivo .dyn.

      As referências de elementos do hospedeiro devem ser salvas no arquivo .dyn para que, quando um usuário abrir um gráfico contendo esse nó, sua seleção ainda esteja definida. Os nós NodeModel no Dynamo usam json.net para serializar. Todas as propriedades públicas serão serializadas automaticamente usando Json.net. Use o atributo [JsonIgnore] para serializar apenas o que for necessário.
* Os nós de consulta de documento são um pouco mais simples, pois não precisam armazenar uma referência a nenhuma ID de elemento. Veja a seguir as implementações da classe `ElementQueryBase` e da classe derivada. Quando executados, esses nós fazem uma chamada para a API do Revit e consultam o documento subjacente em busca de elementos, além de realizar a conversão mencionada anteriormente para a geometria ou os wrappers de elementos do Revit.

### Referências:

#### Classes base do DynamoCore:

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs)
* [Estudo de caso do NodeModel – Interface do usuário personalizada](11_developer_primer/3_developing_for_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Atualizar os pacotes e as bibliotecas do Dynamo para Dynamo 2.x](11_developer_primer/3_developing_for_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Atualizar os pacotes e as bibliotecas do Dynamo para Dynamo 3.x](11_developer_primer/3_developing_for_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)

#### DynamoRevit:

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs)
* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)

## Visão geral dos pacotes integrados do Dynamo

O mecanismo de Pacotes Integrados é um esforço para agrupar mais conteúdo de nós com o Dynamo Core sem expandir o núcleo em si, aproveitando a funcionalidade de carregamento de pacotes do Dynamo implementada pela extensão `PackageLoader` e `PackageManager`.

Neste documento, usaremos alternadamente os termos Pacotes integrados, Pacotes integrados do Dynamo e pacotes integrados para significar a mesma coisa.

### Devo enviar um pacote como um Pacote integrado?

* O pacote deve ter pontos de entrada binários assinados ou não será carregado.
* Todo o esforço deve ser feito para evitar alterações de quebra nesses pacotes. Isso significa que o conteúdo do pacote deve ter testes automatizados.
* Versão semântica: provavelmente é uma boa ideia controlar a versão do pacote usando um esquema de versão semântica e comunicá-lo aos usuários na descrição do pacote ou na documentação.
* Testes automatizados. Veja acima, se um pacote for incluído usando o mecanismo de Pacote integrado, para um usuário, ele parecerá fazer parte do produto e deverá ser testado como um produto.
* Alto nível de polimento: ícones, documentação de nós, conteúdo localizado.
* Não envie pacotes que você ou sua equipe não possam manter.
* Não envie pacotes de terceiros dessa forma (consulte acima).

Basicamente, você deve ter controle total sobre o pacote, a capacidade de corrigi-lo, mantê-lo atualizado e testá-lo em relação às últimas alterações no Dynamo e em seu produto. Você também precisa ser capaz de assiná-lo.

### Pacotes integrados versus pacotes específicos de integração de hospedeiro

Nossa intenção é que o `Built-In Packages` seja um recurso central, um conjunto de pacotes aos quais todos os usuários tenham acesso, mesmo que não tenham acesso ao gerenciador de pacotes. Atualmente, o mecanismo subjacente para suportar esse recurso é uma localização de carregamento padrão adicional para pacotes diretamente no diretório do Dynamo Core, em relação ao DynamoCore.dll.

Com algumas restrições, essa localização poderá ser usada por clientes e integradores do ADSK Dynamo para distribuir seus pacotes específicos de integração _(por exemplo, a integração do FormIt no Dynamo requer um pacote personalizado do FormIt no Dynamo)._

Como o mecanismo de carregamento subjacente é o mesmo para pacotes de núcleo e específicos do hospedeiro, será necessário garantir que os pacotes distribuídos dessa maneira não causem confusão ao usuário sobre pacotes principais `Built-In Packages` versus pacotes específicos de integração que estão disponíveis apenas em um único produto hospedeiro. Recomendamos que, para evitar que o usuário se confunda, os pacotes específicos do hospedeiro sejam introduzidos na discussão com as equipes do Dynamo.

### Localização dos pacotes

Como os pacotes incluídos no `Built-In Packages` estarão disponíveis para mais clientes e as garantias que damos sobre eles serão mais rigorosas (veja acima), eles devem ser localizados.

Para pacotes ADSK internos destinados à inclusão de `Built-In Packages`, as limitações atuais de não poder publicar conteúdo localizado no gerenciador de pacotes não são bloqueadoras, pois os pacotes não precisam necessariamente ser publicados no gerenciador de pacotes.

Usando uma solução alternativa, é possível criar manualmente (e até publicar) pacotes com subdiretórios de cultura na pasta /bin de um pacote.

Primeiro, crie manualmente os subdiretórios específicos da cultura que você precisa na pasta `/bin` dos pacotes.

Se, por algum motivo, o pacote também precisar ser publicado no gerenciador de pacotes, será necessário primeiro publicar uma versão do pacote que não tenha esses subdiretórios de cultura. Depois, publicar uma nova versão do pacote usando o DynamoUI `publish package version`. O carregamento da nova versão no Dynamo não deve excluir as pastas e os arquivos em `/bin`, que você adicionou manualmente usando o explorador de arquivos do Windows. O processo de carregamento de pacotes no Dynamo será atualizado para lidar com os requisitos de arquivos localizados no futuro.

Esses subdiretórios de cultura serão carregados sem problemas pelo tempo de execução .net se estiverem localizados no mesmo diretório que os binários do nó/extensão.

Para obter mais informações sobre as montagens de recursos e os arquivos .resx, consulte: [https://docs.microsoft.com/pt-br/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/pt-br/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

Você provavelmente estará criando os arquivos `.resx` e compilando-os com o Visual Studio. Para uma determinada montagem `xyz.dll` (os recursos resultantes serão compilados em uma nova montagem `xyz.resources.dll`). Conforme descrito acima, a localização e o nome dessa montagem são importantes.

O `xyz.resources.dll` gerado deve estar localizado como segue: `package\bin\culture\xyz.resources.dll`.

Para acessar as sequências de caracteres localizadas no pacote, é possível usar o ResourceManager, mas ainda mais simples: você deve conseguir consultar o `Properties.Resources.YourLocalizedResourceName` de dentro da montagem para a qual você adicionou um arquivo `.resx`. Por exemplo, consulte:

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) para obter um exemplo de mensagem de erro localizada

ou [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) para obter um exemplo de uma sequência de caracteres de atributo NodeDescription específica do Dynamo localizada.

ou [https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) para obter outro exemplo.

### Layout da biblioteca de nós

Normalmente, quando o Dynamo carrega nós de um pacote, ele os coloca na seção `Addons` na biblioteca de nós. Para integrar melhor os nós de pacotes integrados com outros conteúdos integrados, adicionamos a capacidade de os autores de pacotes integrados fornecerem um arquivo `layout specification` parcial para ajudar a colocar os novos nós na categoria de nível superior correta na seção da biblioteca `default`.

Por exemplo, o seguinte arquivo json de especificação de layout, se encontrado no caminho `package/extra/layoutspecs.json`, colocará os nós especificados por `path` na categoria `Revit` na seção `default`, que é a principal seção de nós integrados.

Observe que os nós importados de um pacote interno terão o prefixo `bltinpkg://`, quando forem considerados para correspondência com um caminho incluído na especificação de layout.

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

As modificações complexas de layout não foram bem testadas ou não são suportadas. A intenção para o carregamento dessa especificação de layout em especial é mover um namespace de pacote inteiro sob uma categoria de hospedeiro específica como `Revit` ou `Formit`.
