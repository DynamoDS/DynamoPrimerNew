# Functions

As funções podem ser criadas em um Code Block e chamadas em outro lugar da definição do Dynamo. Isto cria outra camada de controle em um arquivo paramétrico e pode ser visualizado como uma versão com base em texto de um nó personalizado. Neste caso, o Code Block "principal" é facilmente acessível e pode ser localizado em qualquer lugar do gráfico. Não é necessário fio!

### Principal

A primeira linha tem a palavra-chave "def", depois o nome da função e, a seguir, os nomes das entradas entre parênteses. Os contraventamentos definem o corpo da função. Retorne um valor com "return =". Os Code Blocks que definem uma função não possuem portas de entrada ou saída porque eles são chamados a partir de outros Code Blocks.

![](<../images/8-1/4/functions parent def.jpg>)

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

### Filhos

Chame a função com outro Code Block no mesmo arquivo fornecendo o mesmo nome e o mesmo número de argumentos. Funciona da mesma forma que os nós imediatos da sua biblioteca.

![](<../images/8-1/4/functions children call def.jpg>)

```
FunctionName(in1,in2);
```

## Exercício: Esfera por Z

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

Neste exercício, vamos criar uma definição genérica que irá criar esferas a partir de uma lista de entrada de pontos. O raio dessas esferas é conduzido pela propriedade Z de cada ponto.

Vamos começar com um intervalo de números de dez valores que vão de 0 a 100. Conecte-os em um nó **Point.ByCoordinates** para criar uma linha diagonal.

![](<../images/8-1/4/functions - exercise - 01.jpg>)

Crie um **Bloco de código** e insira nossa definição.

![](<../images/8-1/4/functions - exercise - 02.jpg>)

> 1. Use estas linhas de código:
>
>    ```
>    def sphereByZ(inputPt)
>    {
>    
>    };
>    ```
>
> O _inputPt_ é o nome que proporcionamos para representar os pontos que irão guiar a função. A partir de agora, a função não está fazendo nada, mas vamos criar esta função nas etapas a seguir.

![](<../images/8-1/4/functions - exercise - 03.jpg>)

> 1. Adicionando à função **Bloco de código**, colocamos um comentário e uma variável _sphereRadius_ que consulta a posição _Z_ de cada ponto. Lembre-se, _inputPt.Z_ não precisa de parênteses como um método. Esta é uma _query_ das propriedades de um elemento existente, portanto, nenhuma entrada é necessária:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](<../images/8-1/4/functions - exercise - 04.jpg>)

> 1. Agora, vamos recordar a função que criamos em outro **Bloco de código**. Se clicarmos duas vezes na tela para criar um novo _Code Block_ e digitar _sphereB_, notamos que o Dynamo sugere a função _sphereByZ_ que foi definida. Sua função foi adicionada à biblioteca intellisense! Muito legal.

![](<../images/8-1/4/functions - exercise - 05.jpg>)

> 1. Agora, chamamos a função e criamos uma variável chamada _Pt_ para conectar os pontos criados nas etapas anteriores:
>
>    ```
>    sphereByZ(Pt)
>    ```
> 2. Observe que, a partir da saída, temos todos os valores nulos. Por que é isso está acontecendo? Quando definimos a função, estamos calculando a variável _sphereRadius_, mas não definimos o que a função deve _retornar_ como uma _saída_. Podemos corrigir isso na próxima etapa.

![](<../images/8-1/4/functions - exercise - 06.jpg>)

> 1. Uma etapa importante: é necessário definir a saída da função adicionando a linha `return = sphereRadius;` à função _sphereByZ_.
> 2. Agora, vemos que a saída do Bloco de código nos fornece as coordenadas Z de cada ponto.

Agora, vamos criar esferas reais editando a função _Principal_.

![](<../images/8-1/4/functions - exercise - 07.jpg>)

> 1. Primeiro definimos uma esfera com a linha de código: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`
> 2. Em seguida, alteramos o valor de retorno para _sphere_ em vez de _sphereRadius_: `return = sphere;` Isso nos fornece algumas esferas gigantes em nossa visualização do Dynamo.

![](<../images/8-1/4/functions - exercise - 08.jpg>)

> 1\. Para reduzir o tamanho dessas esferas, vamos atualizar o valor sphereRadius adicionando um divisor: `sphereRadius = inputPt.Z/20;` Agora podemos ver as esferas separadas e começar a entender a relação entre raio e valor Z.

![](<../images/8-1/4/functions - exercise - 09.jpg>)

> 1. No nó **Point.ByCoordinates**, alterando a amarra de Lista mais curta para Produto transversal, criamos uma grade de pontos. A função _sphereByZ_ ainda está em pleno efeito, de modo que todos os pontos criam esferas com raios com base em valores Z.

![](<../images/8-1/4/functions - exercise - 10.jpg>)

> 1. E apenas para efeitos de teste, nós conectamos a lista original de números à entrada X para **Point.ByCoordinates**. Agora temos um cubo de esferas.
> 2. Observação: se isso levar muito tempo para ser calculado no seu computador, tente alterar _#10_ para algo como _#5_.

Lembre-se, a função _sphereByZ_ que foi criada é uma função genérica, para que possamos recuperar a hélice de uma lição anterior e aplicar a função a ela.

![](<../images/8-1/4/functions - exercise - 11.jpg>)

Uma etapa final: vamos conduzir a relação do raio com um parâmetro definido pelo usuário. Para fazer isso, é necessário criar uma nova entrada para a função e também substituir o divisor _20_ por um parâmetro.

![](<../images/8-1/4/functions - exercise - 12.jpg>)

> 1. Atualize a definição _sphereByZ_ para:
>
>    ```
>    def sphereByZ(inputPt,radiusRatio)
>    {
>    //get Z Value, use it to drive radius of sphere
>    sphereRadius=inputPt.Z/radiusRatio;
>    //Define Sphere Geometry
>    sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>    //Define output for function
>    return sphere;
>    };
>    ```
> 2. Atualize os **Códigos de bloco** secundários adicionando uma variável de relação à entrada: `sphereByZ(Pt,ratio);` Conecte um controle deslizante para a entrada do **Código de bloco** recém-criado e varie o tamanho dos raios com base na relação de raio.
