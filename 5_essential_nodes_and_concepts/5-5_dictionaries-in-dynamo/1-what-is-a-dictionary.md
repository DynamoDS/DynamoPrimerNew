# O que é um dicionário

O Dynamo 2.0 apresenta o conceito de separar o tipo de dados de dicionários do tipo de dados de listas. Essa alteração pode representar algumas alterações significativas na forma como você cria e trabalha com os dados em seus fluxos de trabalho. Antes da versão 2.0, os dicionários e as listas eram combinados como um tipo de dados. Em resumo, as listas eram, na verdade, dicionários com chaves de número inteiro.

### **O que é um dicionário?**

Um dicionário é um tipo de dados formado por um conjunto de pares de valores-chave onde cada chave é exclusiva em cada conjunto. Um dicionário não tem ordem e, basicamente, é possível “procurar por coisas” usando uma chave em vez de um valor de índice, como em uma lista. _No Dynamo 2.0, as chaves somente podem ser sequências de caracteres._

### **O que é uma lista?**

Uma lista é um tipo de dados formado por um conjunto de valores ordenados. No Dynamo, as listas usam números inteiros como valores de índice.

### **Por que essa mudança foi feita e por que é importante para mim?**

A separação dos dicionários das listas apresenta os dicionários como um elemento de primeira classe que você pode usar para armazenar e pesquisar valores de maneira rápida e fácil, sem precisar lembrar um valor de índice ou manter uma estrutura de lista rigorosa em todo o fluxo de trabalho. Durante o teste dos usuários, vimos uma redução significativa no tamanho do gráfico quando os dicionários eram usados em vez de vários nós `GetItemAtIndex`.

### **Quais são as mudanças?**

* Ocorreram alterações na _sintaxe_ que alteram a forma como você inicializará e trabalhará com dicionários e listas em blocos de código.
  * Os dicionários usam a seguinte sintaxe `{key:value}`
  * As listas usam a seguinte sintaxe `[value,value,value]`
* _Novos nós_ foram introduzidos na biblioteca para ajudar a criar, modificar e consultar dicionários.
*   As listas criadas em blocos de código v1.x migrarão automaticamente durante o carregamento do script com a nova sintaxe de lista que usa colchetes `[ ]` em vez de chaves `{ }` \\

    ***

\![](<../images/5-5/1/what is a dictionary - what are the changes (1) (1) (1).jpg>)

***

### **Por que isso é importante para mim? Para que você usaria isso?**

Em ciência de computação, os dicionários, como as listas, são conjuntos de objetos. Embora as listas estejam em uma ordem específica, os dicionários são conjuntos _não ordenados_. Eles não dependem de números sequenciais (índices); em vez disso, usam _chaves._

Na imagem abaixo, demonstramos um caso de uso potencial de um dicionário. Muitas vezes, os dicionários são usados para relacionar duas partes dos dados que podem não ter uma correlação direta. No nosso caso, estamos ligando a versão em espanhol de uma palavra à versão em inglês para uma pesquisa posterior.

![](../images/5-5/1/whatisadictionary-whatwouldyouusethesefor.jpg)

> 1. Crie um dicionário para relacionar as duas partes dos dados.
> 2. Obtenha o valor com a chave fornecida.
