# Renomear as estruturas

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Ao adicionar tubulações e estruturas a uma rede de tubulação, o Civil 3D usa um modelo para atribuir nomes automaticamente. Normalmente, isso é suficiente durante o posicionamento inicial, mas inevitavelmente os nomes terão que mudar no futuro à medida que o projeto evoluir. Além disso, há muitos padrões de nomenclatura diferentes que podem ser necessários, por exemplo, nomear estruturas sequencialmente em um trecho de tubulação com base na estrutura a jusante mais distante ou seguir um padrão de nomenclatura alinhado com o esquema de dados de um órgão local. Este exemplo demonstrará como o Dynamo pode ser usado para definir qualquer tipo de estratégia de nomeação e aplicá-lo de forma consistente.

## Objetivo

> :dart: Renomear as estruturas de rede de tubulação na ordem com base no estaqueamento de um alinhamento.

## Principais conceitos

> * Trabalhar com caixas delimitadoras
> * Filtrar dados usando o nó **List.FilterByBoolMask**
> * Classificar os dados usando o nó **List.SortByKey**
> * Gerar e modificar sequências de texto

## Compatibilidade de versão

{% hint style="success" %}\r\n Este gráfico será executado no **Civil 3D 2020** e versões superiores. \r\n{% endhint %}

## Conjunto de dados

Comece fazendo o download dos arquivos de amostra abaixo e, em seguida, abrindo o arquivo DWG e o gráfico do Dynamo.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Solução

Apresentamos a seguir uma visão geral da lógica no gráfico.

> 1. Selecionar as estruturas por camada
> 2. Obter as localizações da estrutura
> 3. Filtrar as estruturas por deslocamento e, em seguida, classificá-las por estaca
> 4. Gerar os novos nomes
> 5. Renomear as estruturas

Vamos começar

### Selecionar as estruturas

A primeira coisa que precisamos fazer é selecionar todas as estruturas com as quais planejamos trabalhar. Para fazer isso, basta selecionar todos os objetos em uma determinada camada, o que significa que podemos selecionar estruturas de diferentes redes de tubulação (presumindo que compartilhem a mesma camada).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Seleção das estruturas em uma determinada camada</p></figcaption></figure>

> 1. Esse nó assegura que não recuperemos acidentalmente quaisquer tipos de objetos indesejados que possam compartilhar a mesma camada que as estruturas.

### Obter as localizações da estrutura

Agora que temos as estruturas, precisamos descobrir sua posição no espaço para que possamos classificá-las de acordo com sua localização. Para fazer isso, aproveitaremos a caixa delimitadora de cada objeto. A **Caixa delimitadora** de um objeto é a caixa de tamanho mínimo que contém completamente as extensões geométricas do objeto. Ao calcular o centro da caixa delimitadora, obtemos uma boa aproximação do ponto de inserção da estrutura.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Uso de caixas delimitadoras para obter o ponto de inserção aproximado de cada estrutura</p></figcaption></figure>

Usaremos estes pontos para obter a estaca e o deslocamento das estruturas em relação a um alinhamento selecionado.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtrar e classificar

Neste ponto é que as coisas começam a ficar um pouco complicadas. Nesta fase, temos uma lista grande de todas as estruturas na camada que especificamos e escolhemos um alinhamento que desejamos classificar. O problema é que pode haver estruturas na lista que não queremos renomear. Por exemplo, elas podem não fazer parte do trecho específico em que estamos interessados.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. O alinhamento selecionado
> 2. As estruturas que queremos renomear
> 3. As estruturas que devem ser ignoradas

Portanto, precisamos filtrar a lista de estruturas para que não consideremos as que são maiores do que um determinado deslocamento do alinhamento. Isso é melhor realizado usando o nó **List.FilterByBoolMask**. Após filtrar a lista de estruturas, usamos o nó **List.SortByKey** para classificá-las por seus valores de estaca.

{% hint style="info" %}\r\n Se você não estiver familiarizado com o trabalho com listas, veja a seção [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtragem e classificação das estruturas</p></figcaption></figure>

> 1. Verificar se o deslocamento da estrutura é menor que o valor de limite
> 2. Substituir os valores nulos por _false_
> 3. Filtrar a lista de estruturas e estacas
> 4. Classificar as estruturas pelas estacas

### Gerar os novos nomes

O último trabalho que precisamos fazer é criar os novos nomes para as estruturas. O formato que usaremos é `<alignment name>-STRC-<number>`. Há alguns nós extras aqui para preencher os números com zeros extras, se desejado (por exemplo, “01”em vez de “1”).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Geração dos novos nomes das estruturas</p></figcaption></figure>

### Renomear as estruturas

E, por último, mas não menos importante, renomeamos as estruturas.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Definir os nomes das estruturas</p></figcaption></figure>

### Resultado

Veja um exemplo de como executar o gráfico usando o **Reprodutor do Dynamo**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Execução do gráfico usando o Reprodutor do Dynamo e visualização dos resultados no Civil 3D</p></figcaption></figure>

{% hint style="info" %}\r\n Se o Reprodutor do Dynamo for algo novo para você, veja a seção [dynamo-player.md](../../dynamo-player.md "mention"). \r\n{% endhint %}

> :tada: Missão cumprida.

### Bônus: Visualização no Dynamo

Pode ser útil aproveitar a visualização do plano de fundo 3D do Dynamo para visualizar as saídas intermediárias do gráfico em vez de apenas o resultado final. Uma coisa fácil que podemos fazer é mostrar as caixas delimitadoras das estruturas. Além disso, este conjunto de dados específico tem um corredor no documento; portanto, podemos trazer a geometria da linha de recurso do corredor para o Dynamo para fornecer algum contexto para onde as estruturas estão localizadas no espaço. Se o gráfico for usado em um conjunto de dados que não tem corredores, estes nós simplesmente não farão nada.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Visualização da geometria das estruturas e das linhas de recurso do corredor</p></figcaption></figure>

Agora podemos entender melhor como funciona o processo de filtragem das estruturas por deslocamento.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Ajuste do valor limite de deslocamento do alinhamento e visualização das estruturas afetadas no Dynamo</p></figcaption></figure>

## Ideias

Veja a seguir algumas ideias sobre como você pode expandir os recursos desse gráfico.

{% hint style="info" %}\r\n Renomeie as estruturas com base em seu **Alinhamento mais próximo** em vez de selecionar um alinhamento específico. \r\n{% endhint %}

{% hint style="info" %}\r\n **Renomeie as tubulações** além das estruturas. \r\n{% endhint %}

{% hint style="info" %}\r\n **Defina as camadas** das estruturas com base em seu trecho. \r\n{% endhint %}
