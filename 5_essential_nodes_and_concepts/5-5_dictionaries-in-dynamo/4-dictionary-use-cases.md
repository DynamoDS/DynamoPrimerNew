# Casos de uso do Revit

Alguma vez você já quis procurar algo no Revit por um dado que ele possui?

É provável que você tenha feito algo como o seguinte exemplo.

Na imagem abaixo, estamos coletando todos os ambientes no modelo do Revit, obtendo o índice do ambiente desejado (por número do ambiente) e, por fim, selecionando o ambiente no índice.

![](../images/5-5/4/dictionary-collectroominrevitmodel.jpg)

> 1. Colete todos os ambientes no modelo.
> 2. Número do ambiente a ser localizado.
> 3. Obtenha o número do ambiente e encontre em que índice ele se encontra.
> 4. Obtenha o ambiente no índice.

## Exercício: Dicionário de ambiente

### Parte I: Criar o dicionário de ambiente

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

Agora, vamos recriar essa ideia usando dicionários. Primeiro, precisamos coletar todos os ambientes em nosso modelo do Revit.

![](../images/5-5/4/dictionary-exerciseI-01.jpg)

> 1. Escolhemos a categoria do Revit com a qual queremos trabalhar (neste caso, estamos trabalhando com ambientes).
> 2. Dizemos ao Dynamo para coletar todos esses elementos.

Em seguida, precisamos decidir quais chaves usaremos para examinar esses dados. (Para obter informações sobre as chaves, consulte a seção [O que é um dicionário?](1-what-is-a-dictionary.md)).

![](../images/5-5/4/dictionary-exerciseI-02.jpg)

> 1. Os dados que usaremos são o número do ambiente.

Agora, criaremos o dicionário com as chaves e os elementos indicados.

![](../images/5-5/4/dictionary-exerciseI-03.jpg)

> 1. O nó **Dictionary.ByKeysValues** criará um dicionário com as entradas apropriadas.
> 2. `Keys` precisa ser uma sequência de caracteres, enquanto `values` pode ser uma variedade de tipos de objetos.

Por último, é possível recuperar um ambiente do dicionário com seu número de ambiente atual.

![](../images/5-5/4/dictionary-exerciseI-04.jpg)

> 1. `String` será a chave que vamos usar para pesquisar um objeto no dicionário.
> 2. **Dictionary.ValueAtKey** obterá o objeto do dicionário.

### Parte II: Pesquisa de valores

Usando a mesma lógica de dicionário, também é possível criar dicionários com objetos agrupados. Se quiséssemos examinar todos os ambientes em um determinado nível, poderíamos modificar o gráfico acima, da seguinte forma.

![](../images/5-5/4/dictionary-exerciseII-01.jpg)

> 1. Em vez de usar o número do ambiente como a chave, podemos usar um valor de parâmetro (neste caso, usaremos o nível).

![](../images/5-5/4/dictionary-exerciseII-02.jpg)

> 1. Agora, é possível agrupar os ambientes pelo nível em que residem.

![](../images/5-5/4/dictionary-exerciseII-03.jpg)

> 1. Com os elementos agrupados pelo nível, agora podemos usar as chaves compartilhadas (chaves exclusivas) como a nossa chave para o dicionário e as listas de ambientes como os elementos.

![](../images/5-5/4/dictionary-exerciseII-04.jpg)

> 1. Por fim, usando os níveis no modelo do Revit, podemos procurar quais ambientes residem naquele nível no dicionário. `Dictionary.ValueAtKey` coleta o nome do nível e retorna os objetos do ambiente naquele nível.

As oportunidades de uso do dicionário são realmente infinitas. A capacidade de relacionar seus dados do BIM no Revit com o próprio elemento apresenta uma variedade de casos de uso.
