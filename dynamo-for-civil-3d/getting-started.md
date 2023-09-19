# Guia de Introdução

Agora que você sabe um pouco mais sobre o quadro geral, vamos entrar e criar o primeiro gráfico do Dynamo no Civil 3D.

{% hint style="info" %} Este é um exemplo simples que deve demonstrar a funcionalidade básica do Dynamo. É recomendável seguir as etapas em um novo documento vazio do Civil 3D. {% endhint %}

## Abrir o Dynamo

A primeira coisa a fazer é abrir um documento vazio no Civil 3D. Depois disso, navegue até a guia **Gerenciar** na faixa de opções do Civil 3D e procure o painel **Programação visual**.

\![](<../.gitbook/assets/image (7).png>)

Clique no botão **Dynamo**, que iniciará o Dynamo em uma janela separada.

{% hint style="info" %} **Qual é a diferença entre o Dynamo e o Reprodutor do Dynamo?**

O Dynamo é a ferramenta usada para criar e executar gráficos. O Reprodutor do Dynamo é uma forma fácil de executar gráficos sem precisar abri-los no Dynamo.

Vá para a seção [dynamo-player.md](dynamo-player.md "mention") quando estiver tudo pronto para experimentá-lo. {% endhint %}

## Iniciar um novo gráfico

Quando o Dynamo estiver aberto, você verá a tela inicial. Clique em **Novo** para abrir um espaço de trabalho em branco.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Tela inicial do Dynamo</p></figcaption></figure>

{% hint style="info" %} **E os exemplos?**

O Dynamo for Civil 3D vem com alguns gráficos pré-criados que podem ajudar a estimular mais ideias sobre como você pode usar o Dynamo. Recomendamos que você dê uma olhada neles em algum momento, bem como nos [exemplos de fluxos de trabalho](sample-workflows/ "mention") aqui no manual. {% endhint %}

## Adicionar nós

Agora você deve estar olhando para um espaço de trabalho vazio. Vamos ver o Dynamo em ação. Este é nosso objetivo:

>  :dart: **Criar um gráfico do Dynamo que inserirá texto no espaço do modelo.**

Muito simples, certo? Mas antes de começarmos, precisamos abordar alguns conceitos básicos.

Os blocos de construção principais de um gráfico do Dynamo são chamados de **nós**. Um nó é como uma máquina pequena – você coloca dados nele, ele realiza algum trabalho nesses dados e gera um resultado. O Dynamo for Civil 3D tem uma **biblioteca** de nós que você pode conectar com **fios** para formar um **gráfico** que faz coisas maiores e melhores do que qualquer nó isolado pode fazer.

{% hint style="info" %} **Espere, e se eu nunca tiver usado o Dynamo?**

Alguns desses pontos podem ser novos para você, mas isso não é um problema. Estas seções ajudarão.

[3_user_interface](../3\_user\_interface/ "mention")\
[4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
[5_essencial_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") {% endhint %}

OK, vamos criar nosso gráfico. Veja a seguir uma lista de todos os nós que precisaremos.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

Você pode encontrar esses nós digitando seus nomes na barra de pesquisa na biblioteca ou clicando com o botão direito do mouse em qualquer lugar na tela e pesquisando.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>É possível colocar os nós da biblioteca ou clicando com o botão direito do mouse na tela</p></figcaption></figure>

{% hint style="info" %} **Como posso saber quais nós usar e onde encontrá-los?**

Os nós da biblioteca são agrupados em categorias lógicas com base no que fazem. Dê uma olhada na seção [node-library.md](node-library.md "mention") para ver um tour mais aprofundado. {% endhint %}

Veja a seguir como deve ser a aparência do gráfico final.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>O gráfico finalizado</p></figcaption></figure>

Vamos resumir o que fizemos aqui:

> 1. Escolhemos em qual documento trabalhar. Neste caso (e em muitos casos), queremos trabalhar no documento ativo no Civil 3D.
> 2. Definimos o bloco de destino onde o objeto de texto deve ser criado (neste caso, espaço do modelo).
> 3. Usamos um nó _String_ para especificar em qual camada o texto deve ser colocado.
> 4. Criamos um ponto usando o nó _Point.ByCoordinates_ para definir a posição em que o texto deve ser colocado.
> 5. Definimos as coordenadas X e Y do ponto de inserção de texto usando dois nós _Number Slider_.
> 6. Usamos outro nó _String_ para definir o conteúdo do objeto de texto.
> 7. E, por fim, criamos o objeto de texto.

Vamos ver os resultados do nosso novíssimo gráfico.

## Veja o resultado

De volta ao Civil 3D, assegure-se de que a guia **Modelo** esteja selecionada. Você deve ver o novo objeto de texto que o Dynamo criou.

{% hint style="info" %} Se você não conseguir ver o texto, talvez seja necessário executar o comando ZOOM -> EXTENTS para efetuar o zoom no ponto certo. {% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Legal. Agora, vamos fazer algumas atualizações no texto.

De volta ao gráfico do Dynamo, vá em frente e altere alguns dos valores de entrada, como a sequência de texto, as coordenadas do ponto de inserção etc. Você deve ver o texto automaticamente atualizado no Civil 3D. Observe também que, se você desconectar uma das portas de entrada, o texto será removido. Se você conectar tudo de volta, o texto será criado novamente. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>O gráfico finalizado em ação</p></figcaption></figure>

</div>

{% hint style="info" %} **Por que o Dynamo não insere um novo objeto de texto cada vez que o gráfico é executado?**

Por padrão, o Dynamo “lembrará” os objetos que criar. Se você alterar os valores de entrada do nó, os objetos no Civil 3D serão atualizados em vez de criar objetos novos. Você pode ler mais sobre esse comportamento na seção [object-binding.md](advanced-topics/object-binding.md "mention"). {% endhint %}

> :tada: Missão cumprida.

## Próximas etapas

Este exemplo apenas oferece uma pequena noção do que você pode fazer com o Dynamo for Civil 3D. Continue lendo para saber mais.
