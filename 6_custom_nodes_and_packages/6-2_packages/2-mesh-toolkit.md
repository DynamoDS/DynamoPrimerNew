# Estudo de caso do pacote – Kit de ferramentas de malha

O Kit de ferramentas de malha do Dynamo fornece ferramentas para importar malhas de formatos de arquivos externos, para criar uma malha com base nos objetos de geometria do Dynamo e para criar manualmente malhas de acordo com seus vértices e índices. A biblioteca também fornece ferramentas para modificar malhas, reparar malhas ou extrair camadas horizontais para uso na fabricação.

![](<../images/6-2/2/meshToolkitcasestudy01 (2).jpg>)

O Kit de ferramentas de malha do Dynamo faz parte da pesquisa contínua sobre malhas da Autodesk e, como tal, continuará a crescer nos próximos anos. Pode contar com o aparecimento frequente de novos métodos no kit de ferramentas. Sinta-se à vontade para entrar em contato com a equipe do Dynamo com comentários, bugs e sugestões de novos recursos.

### Malhas vs. sólidos

O exercício abaixo demonstra algumas operações básicas de malha usando o Kit de ferramentas de malha. No exercício, intersecionamos uma malha com uma série de planos, que podem ser computacionalmente caros usando sólidos. Ao contrário de um sólido, uma malha tem uma “resolução” definida e não é definida matematicamente, e sim topologicamente, sendo possível definir essa resolução com base na tarefa em questão. Para obter mais detalhes sobre as relações entre malhas e sólidos, consulte o capítulo [ Geometria do projeto computacional](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/) neste manual. Para uma análise mais completa do Kit de ferramentas de malha, consulte a [página Wiki do Dynamo.](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit) Vamos analisar o pacote no exercício abaixo.

### Instalar o Kit de ferramentas de malha

No Dynamo, vá para _Pacotes > Procurar pacotes..._ na barra do menu superior. No campo de pesquisa, digite _“MeshToolkit”_, tudo junto, com atenção às maiúsculas. Clique em Instalar para iniciar o download. É tão simples quanto isso.

![](../images/6-2/2/meshToolkitcasestudy-installpackage.jpg)

## Exercício: Interseção de malha

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

Neste exemplo, vamos analisar o nó Intersect no kit de ferramentas de malha. Vamos importar uma malha e intersecioná-la com uma série de planos de entrada para criar fatias. Esse é o ponto inicial para preparar o modelo para fabricação de um cortador a laser, um cortador a jato de água ou uma fresa CNC.

Para começar, abra _Mesh-Toolkit_Intersect-Mesh.dyn in Dynamo._

![](../images/6-2/2/meshToolkitcasestudy-exercise01.jpg)

> 1. **Caminho do arquivo:** localize o arquivo de malha a ser importado (_stanford_bunny_tri.obj_). Os tipos de arquivo suportados são .mix e .obj
> 2. **Mesh.ImportFile:** conecte o caminho do arquivo para importar a malha

![](../images/6-2/2/meshToolkitcasestudy-exercise02.jpg)

> 1. **Point.ByCoordinates:** crie um ponto, ele será o centro de um arco.
> 2. **Arc.ByCenterPointRadiusAngle:** crie um arco em torno do ponto. Essa curva será usada para posicionar uma série de planos. __ As configurações são as seguintes: __ `radius: 40, startAngle: -90, endAngle:0`

Crie uma série de planos orientados ao longo do arco.

![](../images/6-2/2/meshToolkitcasestudy-exercise03.jpg)

> 1. **Bloco de código**: crie 25 números entre 0 e 1.
> 2. **Curve.PointAtParameter:** conecte o arco à entrada _“curva”_ e a saída do bloco de código à entrada _“parâmetro”_ para extrair uma série de pontos ao longo da curva.
> 3. **Curve.TangentAtParameter:** conecte as mesmas entradas do nó anterior.
> 4. **Plane.ByOriginNormal:** conecte os pontos à entrada _“origem”_ e os vetores à entrada _“normal”_ para criar uma série de planos em cada ponto.

Em seguida, usaremos esses planos para fazer a interseção com a malha.

![](../images/6-2/2/meshToolkitcasestudy-exercise04.jpg)

> 1. **Mesh.Intersect:** faça a interseção dos planos com a malha importada, criando uma série de contornos de policurvas. Clique com o botão direito do mouse em Nó e defina a amarra como a mais longa
> 2. **PolyCurve.Curves:** divida as policurvas em seus fragmentos de curva.
> 3. **Curve.EndPoint:** extraia os pontos finais de cada curva.
> 4. **NurbsCurve.ByPoints:** use os pontos para criar uma curva Nurbs. Use um nó Booleano definido como _True_ para fechar as curvas.

Antes de continuar, desative a visualização de alguns dos nós, como: Mesh.ImportFile, Curve.EndPoint, Plane.ByOriginNormal e Arc.ByCenterPointRadiusAngle para ver melhor o resultado.

![](../images/6-2/2/meshToolkitcasestudy-exercise05.jpg)

> 1. **Surface.ByPatch:** crie as correções de superfícies para cada contorno para criar “fatias” da malha.

Adicione um segundo conjunto de fatias para obter um efeito waffle/caixa de ovos.

![](../images/6-2/2/meshToolkitcasestudy-exercise06.jpg)

Você pode ter notado que as operações de intersecção são calculadas mais rapidamente com uma malha do que com um sólido comparável. Fluxos de trabalho como o demonstrado neste exercício ajudam a trabalhar com malhas.
