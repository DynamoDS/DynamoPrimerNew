# Exemplos

Se você estiver procurando exemplos sobre como desenvolver para o Dynamo, consulte os recursos abaixo:

#### Repositórios de amostras <a href="#sample-repositories" id="sample-repositories"></a>

Essas amostras são modelos do Visual Studio que você pode usar para iniciar seu próprio projeto:

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** modelo para os nós Sem toque básicos.
  * Retornar várias saídas: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Usar um objeto de geometria nativo do Dynamo: [Código](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * Exemplo de propriedade (nó de consulta): [Código](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** modelos para os nós NodeModel básicos e personalização da vista.
  * Modelo básico do NodeModel: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Definir atributos de nó (nomes de entrada/saída, descrições, tipos): [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Retornar um nó nulo se não houver entradas: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Criar uma chamada de função: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Modelo básico de personalização da vista do NodeModel: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Alertar a interface do usuário que um elemento precisa ser atualizado: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Personalizar o NodeModel: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Definir os atributos do controle deslizante: [Código](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Determinar a lógica de interação do controle deslizante: [Código](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** modelos para o nó Sem toque, interface do usuário personalizada, testes e extensões de vista.
  * [Amostras de interface do usuário](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Criar um nó de interface de usuário básico e personalizado: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * Criar um menu suspenso: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [Testes](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Testes do sistema: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * Testes do nó Sem toque: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [Exemplos de nós Sem toque](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * Exemplo de nós Sem toque, incluindo um que implementa `IGraphicItem` para afetar a renderização da geometria: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * Exemplo de nós Sem toque para colorir a geometria usando `IRenderPackage`: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Exemplos de extensão de vista](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): uma implementação IViewExtension que mostra uma janela sem janela restrita quando seu MenuItem é clicado.
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** modelos para desenvolvimento avançado de pacotes do Dynamo usando o NodeModel.
  * Amostras essenciais:
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Amostras de geometria:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Amostras de interface do usuário:
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopiableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Slider](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**:** uma biblioteca de nós Sem toque para criar texto no Dynamo.

#### Estudos de caso <a href="#case-studies" id="case-studies"></a>

Os desenvolvedores terceiros fizeram contribuições significativas e empolgantes para a plataforma, muitas das quais também são de código aberto. Os projetos a seguir são exemplos excepcionais do que pode ser feito com o Dynamo.

O **Ladybug** é uma biblioteca Python para carregar, analisar e modificar arquivos EnergyPlus Weather (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** é uma biblioteca Python para criar, executar e visualizar os resultados da análise de luz natural (RADIANCE) e energia (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee**: um plug-in para interoperabilidade do Excel e do Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** é uma coleção de nós personalizados para atividades relacionadas ao Revit, bem como outras finalidades, como gerenciamento de lista, operações matemáticas, operações de sequência de caracteres, operações geométricas (principalmente caixas delimitadoras, malhas, planos, pontos, superfícies, UVs e vetores) e painéis.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
