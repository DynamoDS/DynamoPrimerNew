# Posicionamento de serviço

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

O projeto de engenharia de uma construção habitacional típica envolve o trabalho com várias concessionárias subterrâneas, como esgoto sanitário, drenagem de águas pluviais, água potável ou outras. Este exemplo demonstrará como o Dynamo pode ser usado para desenhar as conexões de serviço de uma linha principal de distribuição até um determinada subdivisão de propriedade (por exemplo, um lote). É comum que todos os lotes exijam uma conexão de serviço, o que introduz um trabalho tedioso significativo para colocar todos os serviços. O Dynamo pode acelerar o processo desenhando automaticamente a geometria necessária com precisão, além de fornecer entradas flexíveis que podem ser ajustadas para se adequarem às normas de órgãos locais.

## Objetivo

> :dart: Colocar referências de bloco do medidor de serviço de água em deslocamentos especificados de uma linha do lote e desenhar uma linha para cada conexão de serviço perpendicular à linha principal de distribuição.

## Principais conceitos

> * Usar o nó **Selecionar objeto** para a entrada do usuário
> * Trabalhar com sistemas de coordenadas
> * Usar operações geométricas como **Geometry.DistanceTo** e **Geometry.ClosestPointTo**
> * Criar referências de bloco
> * Controlar as configurações de vinculação de objetos

## Compatibilidade de versão

{% hint style="success" %}\r\n Este gráfico será executado no **Civil 3D 2020** e versões superiores. \r\n{% endhint %}

## Conjunto de dados

Comece fazendo o download dos arquivos de amostra abaixo e, em seguida, abrindo o arquivo DWG e o gráfico do Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Solução

Apresentamos a seguir uma visão geral da lógica no gráfico.

> 1. Obter a geometria da curva para a linha principal de distribuição
> 2. Obter a geometria da curva para uma linha do lote selecionada pelo usuário, revertendo se necessário
> 3. Gerar os pontos de inserção para os medidores de serviço
> 4. Obter os pontos na linha principal de distribuição que estão mais próximos das localizações do medidor de serviço
> 5. Criar referências de bloco e linhas no espaço do modelo

Vamos começar

### Obter a geometria principal de distribuição

Nossa primeira etapa é obter a geometria da linha de distribuição principal no Dynamo. Em vez de selecionar linhas individuais ou polilinhas, vamos obter todos os objetos em uma determinada camada e uni-los como uma PolyCurve do Dynamo.

{% hint style="info" %}\r\n Se a geometria da curva do Dynamo for nova para você, veja a seção [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Obtenção dos objetos do Civil 3D e união de tudo em uma única PolyCurve</p></figcaption></figure>

### Obter a geometria da linha do lote

Em seguida, precisamos colocar a geometria de uma linha do lote selecionada no Dynamo para que possamos trabalhar com ela. A ferramenta certa para o trabalho é o nó **Selecionar objeto**, que permite que o usuário do gráfico selecione um objeto específico no Civil 3D.

Também precisamos lidar com um possível problema que surja. A linha do lote tem um ponto inicial e um ponto final, o que significa que ela tem uma direção. Para que o gráfico produza resultados consistentes, precisamos que todas as linhas do lote tenham uma direção consistente. Podemos levar em conta essa condição diretamente na lógica do gráfico, o que torna o gráfico mais resiliente. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Seleção de uma linha do lote e certificação de que ela tenha a direção correta</p></figcaption></figure>

> 1. Obtenha os pontos inicial e final da linha do lote.
> 2. Meça a distância de cada ponto até a linha principal de distribuição e, em seguida, descubra qual distância é maior.
> 3. O resultado desejado é que o ponto inicial da linha seja o mais próximo da linha principal de distribuição. Se esse não for o caso, invertemos a direção da linha do lote. Caso contrário, simplesmente retornaremos a linha do lote original.

### Gerar pontos de inserção

É hora de descobrir onde os medidores de serviço serão colocados. Normalmente, a colocação é determinada pelos requisitos de órgãos locais; portanto, forneceremos valores de entrada que podem ser alterados para atender a várias condições. Vamos usar um **Sistema de coordenadas** ao longo da linha do lote como referência para criar os pontos. Isso torna muito fácil a definição de deslocamentos relativos à linha do lote, não importando sua orientação.

{% hint style="info" %}\r\n Se os sistemas de coordenadas forem algo novo para você, veja a seção [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Criação de pontos de inserção para os medidores de serviço</p></figcaption></figure>

### Obter os pontos de conexão

Agora precisamos obter os pontos na linha principal de distribuição que estão mais próximos da localizações do medidor de serviço Isso nos permitirá desenhar as conexões de serviço no espaço do modelo para que elas sempre sejam perpendiculares à linha principal de distribuição. O nó **Geometry.ClosestPointTo** é a solução perfeita.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Obtenção de pontos perpendiculares na linha principal de distribuição</p></figcaption></figure>

> 1. Essa é a PolyCurve da linha principal de distribuição
> 2. Esses são os pontos de inserção do medidor de serviço

### Criar objetos

A última etapa é efetivamente criar objetos no espaço do modelo. Usaremos os pontos de inserção que geramos anteriormente para criar referências de bloco e, em seguida, usaremos os pontos na linha principal de distribuição para desenhar linhas para as conexões de serviço.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Resultado

Quando você executar o gráfico, deverá ver novas referências de bloco e linhas de conexão de serviço no espaço do modelo. Tente alterar algumas das entradas e veja como tudo é atualizado automaticamente.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Ajuste dos parâmetros de entrada no Dynamo e visualização imediata dos resultados no Civil 3D</p></figcaption></figure>

### Bônus: ativar a colocação sequencial

Você pode observar que, após colocar os objetos para uma linha do lote, selecionar uma linha do lote diferente resulta na “movimentação” dos objetos.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Comportamento quando a vinculação de objetos está ativada</p></figcaption></figure>

Esse é o comportamento padrão do Dynamo e é muito útil em muitos casos. No entanto, você pode achar conveniente colocar várias conexões de serviço sequencialmente e fazer com que o Dynamo crie novos objetos a cada execução em vez de modificar os originais. É possível controlar esse comportamento ao alterar as configurações de vinculação de objetos.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Configurações da vinculação de objetos do Dynamo</p></figcaption></figure>

{% hint style="info" %}\r\n Veja a seção [object-binding.md](../../advanced-topics/object-binding.md "mention") para obter mais informações. \r\n{% endhint %}

Alterar essa configuração forçará o Dynamo a “esquecer” os objetos que cria a cada execução. Veja a seguir um exemplo de como executar o gráfico com a vinculação de objetos desativada usando o **Reprodutor do Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Execução do gráfico usando o Reprodutor do Dynamo e visualização dos resultados no Civil 3D</p></figcaption></figure>

{% hint style="info" %}\r\n Se o Reprodutor do Dynamo for algo novo para você, veja a seção [dynamo-player.md](../../dynamo-player.md "mention"). \r\n{% endhint %}

> :tada: Missão cumprida.

## Ideias

Veja a seguir algumas ideias sobre como você pode expandir os recursos desse gráfico.

{% hint style="info" %}\r\n Coloque **várias conexões de serviço** simultaneamente em vez de selecionar cada linha do lote. \r\n{% endhint %}

{% hint style="info" %}\r\n Ajuste as entradas para colocar **limpeza de esgoto** em vez de medidores de serviço de água. \r\n{% endhint %}

{% hint style="info" %}\r\n **Adicione um botão de alternância** para permitir a colocação de uma única conexão de serviço em um lado específico da linha do lote em vez de em ambos os lados. \r\n{% endhint %}
