# Functions

As funções podem ser criadas em um Code Block e chamadas em outro lugar da definição do Dynamo. Isto cria outra camada de controle em um arquivo paramétrico e pode ser visualizado como uma versão com base em texto de um nó personalizado. Neste caso, o Code Block "principal" é facilmente acessível e pode ser localizado em qualquer lugar do gráfico. Não é necessário fio!

### Principal

A primeira linha tem a palavra-chave "def", depois o nome da função e, a seguir, os nomes das entradas entre parênteses. Os contraventamentos definem o corpo da função. Retorne um valor com "return =". Os Code Block que definem uma função não possuem portas de entrada ou saída porque eles são chamados de outros Code Blocks.

\![](<../../.gitbook/assets/functions parent def.jpg>)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Secundários

Chame a função com outro Code Block no mesmo arquivo fornecendo o mesmo nome e o mesmo número de argumentos. Funciona da mesma forma que os nós prontos para uso da biblioteca.

\![](<../../.gitbook/assets/functions children call def.jpg>)

```
FunctionName(in1,in2);
```

## Exercício: Esfera por Z

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../../.gitbook/assets/Functions_SphereByZ.dyn" %}

Neste exercício, vamos criar uma definição genérica que criará esferas com base em uma lista de entrada de pontos. O raio dessas esferas é conduzido pela propriedade Z de cada ponto.

Vamos começar com um intervalo de números de dez valores , de 0 a 100. Conecte-os a um nó **Point.ByCoordinates** para criar uma linha diagonal.

\![](<../../.gitbook/assets/functions - exercise - 01.jpg>)

Crie um **Bloco de código** e insira a nossa definição.

\![](<../../.gitbook/assets/functions - exercise - 02.jpg>)

> 1.  Use estas linhas de código:
>
>     ```
>     def sphereByZ(inputPt)
>     {
>
>     };
>     ```
>
> O _inputPt_ é o nome que atribuímos para representar os pontos que guiarão a função. Por agora, a função não está fazendo nada, mas vamos desenvolver essa função nas etapas a seguir.

\![](<../../.gitbook/assets/functions - exercise - 03.jpg>)

> 1. Quando a função do **Code Block** é adicionada, colocamos um comentário e uma variável _sphereRadius_ que consulta a posição _Z_ de cada ponto. Lembre-se de que _inputPt.Z_ não precisa de parênteses como um método. Essa é uma _consulta_ das propriedades de um elemento existente; portanto, nenhuma entrada é necessária:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

\![](<../../.gitbook/assets/functions - exercise - 04.jpg>)

> 1. Agora, vamos recordar a função que criamos em outro **Code Block**. Se clicarmos duas vezes na tela para criar um novo _bloco de código_ e digitarmos _sphereB_, notamos que o Dynamo sugere a função _sphereByZ_ que foi definida. A função foi adicionada à biblioteca intellisense. Muito legal.

\![](<../../.gitbook/assets/functions - exercise - 05.jpg>)

> 1.  Agora, chamamos a função e criamos uma variável chamada _Pt_ para conectar os pontos criados nas etapas anteriores:
>
>     ```
>     sphereByZ(Pt)
>     ```
> 2. Observe que, na saída, temos todos os valores nulos. Por que isso ocorre? Quando definimos a função, estamos calculando a variável _sphereRadius_, mas não definimos o que a função deve ser _retornada_ como uma _saída_. Podemos corrigir isso na próxima etapa.

\![](<../../.gitbook/assets/functions - exercise - 06.jpg>)

> 1. Uma etapa importante: é necessário definir a saída da função adicionando a linha `return = sphereRadius;` à função _sphereByZ_.
> 2. Agora, vemos que a saída do Bloco de código nos fornece as coordenadas Z de cada ponto.

Agora, vamos criar esferas reais editando a função _Principal_.

\![](<../../.gitbook/assets/functions - exercise - 07.jpg>)

> 1. Primeiro definimos uma esfera com a linha de código: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`
> 2. Em seguida, alteramos o valor de retorno para _sphere_ em vez de _sphereRadius_: `return = sphere;` Isso nos fornece algumas esferas gigantes em nossa visualização do Dynamo.

\![](<../../.gitbook/assets/functions - exercise - 08.jpg>)

> 1\. Para reduzir o tamanho dessas esferas, vamos atualizar o valor sphereRadius adicionando um divisor: `sphereRadius = inputPt.Z/20;` Agora podemos ver as esferas separadas e começar a entender a relação entre o raio e o valor Z.

\![](<../../.gitbook/assets/functions - exercise - 09.jpg>)

> 1. No nó **Point.ByCoordinates**, alterando a amarra de Lista mais curta para Produto transversal, criamos um eixo de pontos. A função _sphereByZ_ ainda está em pleno efeito, de modo que todos os pontos criam esferas com raios com base em valores Z.

\![](<../../.gitbook/assets/functions - exercise - 10.jpg>)

> 1. E apenas para efeitos de teste, conectamos a lista original de números à entrada X para **Point.ByCoordinates**. Agora temos um cubo de esferas.
> 2. Observação: Se isso levar muito tempo para ser calculado no computador, tente alterar _\#10_ para algo como _\#5_.

Lembre-se: a função _sphereByZ_ que foi criada é uma função genérica, para que possamos recuperar a hélice de uma lição anterior e aplicar a função a ela.

\![](<../../.gitbook/assets/functions - exercise - 11.jpg>)

Uma etapa final: vamos conduzir a relação do raio com um parâmetro definido pelo usuário. Para fazer isso, é necessário criar uma nova entrada para a função e também substituir o divisor _20_ por um parâmetro.

\![](<../../.gitbook/assets/functions - exercise - 12.jpg>)

> 1.  Atualize a definição de _sphereByZ_ para:
>
>     ```
>     def sphereByZ(inputPt,radiusRatio)
>     {
>     //get Z Value, use it to drive radius of sphere
>     sphereRadius=inputPt.Z/radiusRatio;
>     //Define Sphere Geometry
>     sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>     //Define output for function
>     return sphere;
>     };
>     ```
> 2. Atualize o **Bloco de código** secundário adicionando uma variável de relação à entrada: `sphereByZ(Pt,ratio);` Conecte um controle deslizante à entrada do **Bloco de código** recém-criado e varie o tamanho dos raios com base na relação de raio.
