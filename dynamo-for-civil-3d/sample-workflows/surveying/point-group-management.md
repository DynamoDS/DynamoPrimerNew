# Gerenciamento de grupos de pontos

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Trabalhar com pontos COGO e grupos de pontos no Civil 3D é um elemento central de muitos processos do campo à conclusão. O Dynamo realmente brilha quando se trata de gerenciamento de dados e demonstraremos um caso de uso potencial neste exemplo.  

## Objetivo

> :dart: Criar um grupo de pontos para cada descrição de ponto COGO exclusiva. 

## Principais conceitos

> * Trabalhar com listas
> * Agrupar objetos similares com o nó **List.GroupByKey**
> * Mostrar a saída personalizada no Reprodutor do Dynamo

## Compatibilidade de versão

{% hint style="success" %} Este gráfico será executado no **Civil 3D 2020** e versões superiores. {% endhint %}

## Conjunto de dados

Comece fazendo o download dos arquivos de amostra abaixo e, em seguida, abrindo o arquivo DWG e o gráfico do Dynamo.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Solução

Apresentamos a seguir uma visão geral da lógica no gráfico.

> 1. Obter todos os pontos COGO no documento
> 2. Agrupar os pontos COGO por descrição
> 3. Criar os grupos de pontos
> 4. Gerar a saída de um resumo para o Reprodutor do Dynamo

Vamos começar

### Obter os pontos COGO

Nossa primeira etapa é obter todos os grupos de pontos no documento e, em seguida, obter todos os pontos COGO dentro de cada grupo. Isso nos fornecerá uma _lista aninhada_ ou uma “lista de listas” com a qual será mais fácil trabalhar mais tarde se aplainarmos tudo em uma única lista com o nó **List.Flatten**.

{% hint style="info" %} Se você não estiver familiarizado com o trabalho com listas, veja a seção [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Obter todos os grupos de pontos e pontos COGO </p></figcaption></figure>

### Agrupar os pontos por descrição

Agora que temos todos os pontos COGO, precisamos separá-los em grupos com base em suas descrições. É exatamente isso que o nó **List.GroupByKey** faz. Ele essencialmente agrupa todos os itens que compartilham a mesma chave.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Agrupar os pontos COGO por descrição</p></figcaption></figure>

### Criar os grupos de pontos

O trabalho árduo está feito. A etapa final é criar novos grupos de pontos do Civil 3D com base em pontos COGO agrupados.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Criar novos grupos de pontos</p></figcaption></figure>

### Resumo da saída

Quando você executa o gráfico, não há nada para ver na visualização do plano de fundo do Dynamo porque não estamos trabalhando com nenhuma geometria. Portanto, a única maneira de ver se o gráfico foi executado corretamente é verificar o espaço da ferramenta ou analisar as visualizações de saída do nó. No entanto, se executarmos o gráfico usando o **Reprodutor do Dynamo**, poderemos fornecer mais feedback sobre os resultados do gráfico, gerando um resumo dos grupos de pontos que foram criados. Tudo o que você precisa fazer é clicar com o botão direito do mouse em um nó e defini-lo como _É saída_. Neste caso, usamos um nó **Inspeção** renomeado para visualizar os resultados.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>Definir um nó como <em>É saída</em> exibirá seu conteúdo na saída do Reprodutor do Dynamo</p></figcaption></figure>

### Resultado

Veja um exemplo de como executar o gráfico usando o **Reprodutor do Dynamo**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Execução do gráfico usando o Reprodutor do Dynamo e visualização dos resultados no espaço da ferramenta</p></figcaption></figure>

{% hint style="info" %} Se o Reprodutor do Dynamo for algo novo para você, veja a seção [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Missão cumprida.

## Ideias

Veja a seguir algumas ideias sobre como você pode expandir os recursos desse gráfico.

{% hint style="info" %} Modifique o agrupamento de pontos para que seja baseado na **descrição completa** em vez de na descrição original. {% endhint %}

{% hint style="info" %} Agrupe os pontos em algumas outras **categorias predefinidas** que você escolher (por exemplo, “Instantâneos no nível do solo”, “Monumentos” etc.) {% endhint %}

{% hint style="info" %} Cria automaticamente superfícies TIN para pontos em determinados grupos. {% endhint %}
