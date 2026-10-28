# Miniaturas de gráficos do Início do Dynamo

O Início do Dynamo exibe miniaturas de gráficos em um cartão largo. O cartão tem proporção 16:9. O Início do Dynamo centraliza a imagem e corta as partes superior e inferior.

Uma miniatura quadrada pode parecer completa no projeto generativo, que mostra o quadrado inteiro. Porém, essa mesma imagem pode perder títulos ou logotipos quando exibida no Início do Dynamo.

Isso se aplica a qualquer gráfico (`.dyn`), não apenas gráficos dentro de um pacote. Defina a imagem em Propriedades do gráfico. A imagem é armazenada no arquivo do gráfico.

{% hint style="info" %}
Mantenha os títulos, os logotipos e a imagem principal no centro da **área segura**. Use a parte superior e inferior da miniatura quadrada somente como plano de fundo.
{% endhint %}

## Modelos de miniatura

Baixe um template e projete com base nele. A faixa central mais escura corresponde à área exibida no Início do Dynamo. As faixas mais claras, na parte superior e inferior, são cortadas no Início do Dynamo. O projeto generativo mostra o quadrado inteiro.

> Baixe o template SVG para editá-lo em um aplicativo vetorial ou o template PNG para colocar elementos gráficos em um editor de imagens.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Template de miniatura quadrada. A faixa central mais escura fica visível no Início do Dynamo e as faixas mais claras, na parte superior e inferior, são cortadas, mas permanecem visíveis no projeto generativo."><figcaption><p>A faixa central mais escura fica visível no Início do Dynamo. As faixas mais claras, na parte superior e inferior, são cortadas</p></figcaption></figure>

## Criar a miniatura

1. Abra o template SVG ou PNG em um editor de imagens.
2. Coloque os elementos gráficos no quadrado rosa do template.
3. Mantenha o título, o logotipo e a imagem principal dentro da faixa central mais escura.
4. Use as faixas mais claras, na parte superior e inferior, apenas para a cor de fundo ou para os elementos gráficos adicionais.
5. Oculte ou exclua o guia rosa e exporte como PNG **quadrado**.

Use uma imagem **quadrada** (1:1). O Início do Dynamo sempre exibe a faixa central mais escura em um recorte de 16:9. O tamanho dos pixels não altera o recorte. Se você começar com um arquivo em branco, em vez do template, crie-o no formato quadrado. O template de miniaturas em si tem 840 × 840 pixels.

## Adicionar a miniatura a um gráfico

O Início do Dynamo lê a miniatura do arquivo gráfico (`.dyn`), e não do formulário de publicação do Package Manager.

1. Abra o gráfico no Dynamo.
2. Acesse **Arquivo > Mostrar propriedades do gráfico > Geral**.
3. Adicione o PNG exportado no campo de imagem.
4. Salve o gráfico.

Se o gráfico fizer parte de um pacote, inclua o arquivo `.dyn` ao publicar. Consulte [Publicar um pacote](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Verificar o resultado no Início do Dynamo

1. Abra o Início do Dynamo,
2. Em Recente, localize o gráfico que você acabou de salvar.
3. Confirme se o título e o logotipo estão totalmente visíveis no cartão.

Se algum texto estiver cortado, mova-o para mais perto do centro, exporte a imagem novamente e atualize as propriedades do gráfico. Não confie apenas no design generativo. O projeto generativo mostra o quadrado inteiro.
