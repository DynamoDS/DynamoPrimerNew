# Expectativas de testes

Esta página descreve o que buscamos ao testar o novo código que está sendo adicionado ao Dynamo.

Então. Você tem um novo nó que quer adicionar. Ótimo. É hora de adicionar alguns testes. Há dois principais motivos para isso:

1. Ajuda a descobrir onde não funciona
2. Quando alguém altera algo que quebra o nó, isso deve quebrar os testes. Dessa forma, a pessoa que quebrou os testes precisa corrigi-los. Se isso não quebrar os testes, será em grande parte seu problema lidar com os usuários cujos modelos foram quebrados.

Os testes no Dynamo vêm em dois tipos gerais: testes de unidade e testes de sistema.

## Testes de unidade

Os testes de unidade devem testar o mínimo possível. Se você construiu um nó que calcula raízes quadradas por meio de um nó sem toque em C#, o teste deverá testar apenas esse DLL e fazer chamadas em C# diretamente para esse código. Os testes de unidade nem deveriam incluir o Dynamo.

Deveriam incluir:

* Testes positivos (faz a coisa certa)
* Testes negativos (não falha quando recebe entradas inválidas)
* Testes de regressão (quando alguém encontrar um bug no código, escreva um teste para garantir que ele não ocorra novamente)

Eles devem ser pequenos, rápidos e confiáveis. A maioria dos testes deve ser testes de unidade.

## Testes de sistema

Os testes de sistema operam em vários componentes e testam como eles se encaixam. Eles devem ser usados e criados com cuidado. 

Por quê? É caro executá-los. Precisamos manter o conjunto de testes em execução com o mínimo de latência possível.

Ao escrever um teste de sistema, a primeira coisa a fazer é analisar [este](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagrama e descobrir quais subsistemas você está cobrindo.

O ideal seria que houvesse uma série progressiva de testes cobrindo conjuntos crescentes de blocos interativos para que, quando os testes começassem a falhar, fosse muito rápido descobrir onde está o problema.

Exemplos de coisas que precisam de testes de sistema:

* Um novo tipo de nó Revit que armazena vários elementos no rastreamento em vez de um único elemento
* Um novo nó de inspeção que exibe dados de forma diferente

Exemplo de coisas que não precisam de testes de sistema:

* Um novo nó matemático
* Uma biblioteca de processamento de sequência de caracteres

Os testes de sistema devem:

* Afirmar o comportamento correto
* Afirmar ausência de comportamentos patológicos, por exemplo, sem exceções