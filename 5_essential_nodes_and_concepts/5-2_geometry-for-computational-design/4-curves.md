# Curvas

## Curvas no Dynamo

### O que é uma curva?

As [curvas](5-4\_curves.md#curve) são o primeiro tipo de dados geométricos que abordamos que possui um conjunto mais familiar de propriedades descritivas de forma: quão curvas ou retas são? Longas ou curtas? E lembre-se de que os Pontos ainda são os nossos blocos de construção para definir qualquer coisa, de uma linha para uma spline e todos os tipos de curva entre eles.

![Tipos de curvas](../images/5-2/4/CurveTypes.jpg)

> 1. Linha
> 2. Polilinha
> 3. Arco
> 4. Circle
> 5. Elipse
> 6. Curva NURBS
> 7. PolyCurve

### Linha

[Linha](5-4\_curves.md#lines) é composta por um conjunto de pontos, cada linha tem pelo menos 2 pontos. Uma das formas mais comuns de criar linhas no Dynamo é usar `Line.ByStartPointEndPoint` ![](<./images/5-2/4/Line by start point end point.jpg>) para criar uma linha no Dynamo.

![](<./images/5-2/4/curves - line by start point end point.jpg>)

### Curva NURBS

[NURBS](5-4\_curves.md#nurbs-+-polycurves) é um modelo usado para representar curvas e superfícies com precisão. Uma curva senoidal no Dynamo usando dois métodos diferentes para criar curvas NURBS para comparar os resultados.

![](<../images/5-2/4/curves - Nurbs Curves.jpg>)

> 1. _NurbsCurve.ByControlPoints_ utiliza a lista de pontos como pontos de controle
> 2. _NurbsCurve.ByPoints_ desenha uma curva através da lista de pontos

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-2/4/Geometry for Computational Design - Curves.dyn" %}

## Análise abrangente de...

### Curvas

O termo **Curva** é um termo mais abrangente que engloba todas as diferentes formas curvas (mesmo se retas). A curva de capital "C" é a categorização principal para todos esses tipos de forma: linhas, círculos, splines, etc. Tecnicamente, uma curva descreve cada ponto possível que possa ser encontrado inserindo “t” em uma coleção de funções, que podem variar desde funções simples (`x = -1.26*t, y = t`) a funções que envolvam cálculos. Não importa com que tipo de curva estamos trabalhando, este **Parâmetro** chamado "t" é uma propriedade que podemos avaliar. Além disso, independentemente da aparência da forma, todas as curvas também têm um ponto inicial e um ponto final, que coincidem com os valores mínimo e máximo de "t" utilizados para criar a curva. Isso também nos ajuda a compreender sua direcionalidade.

![Parâmetro de curva](../images/5-2/4/CurveParameter.jpg)

> É importante observar que o Dynamo assume que o domínio dos valores "t" para uma curva é compreendido entre 0,0 e 1,0.

Todas as curvas também possuem diversas propriedades ou características que podem ser usadas para descrever ou analisar. Quando a distância entre os pontos inicial e final for zero, a curva será "fechada". Além disso, cada curva tem vários pontos de controle, se todos esses pontos estão localizados no mesmo plano, a curva é "plana". Algumas propriedades se aplicam à curva como um todo, enquanto outras somente se aplicam a pontos específicos ao longo da curva. Por exemplo, a planaridade é uma propriedade global, enquanto um vetor tangente em um determinado valor "t" é uma propriedade local.

### Linhas

**Linhas** são a forma mais simples de Curvas. Podem não parecer curvas, mas na verdade são: apenas sem qualquer curvatura. Existem algumas maneiras diferentes de criar Linhas, sendo a mais intuitiva do ponto A ao ponto B. A forma da linha AB será desenhada entre os pontos, mas matematicamente ela se estende infinitamente em ambas as direções.

![Linha](../images/5-2/4/Line.jpg)

Quando nós conectamos duas linhas juntas, temos uma **Polilinha**. Aqui temos uma representação direta do que é um Ponto de controle. Editar qualquer uma dessas localizações de ponto irá alterar a forma da D. Se a polilinha estiver fechada, temos um polígono. Se os comprimentos de aresta do polígono são todos iguais, ele é descrito como normal.

![Polilinha + Polígono](../images/5-2/4/Polyline.jpg)

### Arcos, círculos, arcos de elipse e elipses

À medida que adicionamos mais complexidade às funções paramétricas que definem uma forma, podemos avançar mais um passo a partir de uma linha para criar um **Arco**, **Círculo**, **Arco da elipse** ou **Elipse** descrevendo um ou dois raios. As diferenças entre a versão do arco e o círculo ou elipse são apenas se a forma está fechada.

![Arcos + Círculos](../images/5-2/4/Arcs+Circles.jpg)

### NURBS + Polilinhas

**NURBS** (Splines base racional não uniforme) são representações matemáticas que podem modelar com precisão qualquer forma de uma linha simples bidimensional, círculo, arco ou retângulo para a curva orgânica tridimensional mais complexa. Devido à sua flexibilidade (relativamente poucos pontos de controle, mas interpolação suave com base nas configurações de grau) e a precisão (delimitado por uma matemática robusta), os modelos NURBS podem ser usados em qualquer processo, da ilustração e da animação à fabricação.

![Curva NURBS](../images/5-2/4/NURBScurve.jpg)

**Grau**: o grau da curva determina o intervalo de influência que os pontos de controle têm em uma curva; onde quanto maior for o grau, maior será o intervalo. O grau é um número inteiro positivo. Este número é normalmente 1, 2, 3 ou 5, mas pode ser qualquer número inteiro positivo. As linhas e polilinhas NURBS são normalmente o grau 1 e a maioria das curvas de forma livre são os graus 3 ou 5.

**Pontos de controle**: os pontos de controle são uma lista de ao menos pontos de graus + 1. Uma das formas mais fáceis de alterar a forma de uma curva NURBS é mover seus Pontos de controle.

**Peso**: os pontos de controle têm um número associado denominado Peso. Os pesos são normalmente números positivos. Quando os Pontos de controle de uma curva têm o mesmo peso (normalmente 1), a Curva é chamada não racional, caso contrário, a Curva é chamada racional. A maioria das curvas NURBS é não racional.

**Nós**: os nós são uma lista de números (graus+N-1), onde N é o número de pontos de controle. Os nós são utilizados junto com os pesos para controlar a influência dos Pontos de controle na curva resultante. Um uso para nós é criar pontos de inflexão em certos pontos da curva.

![Grau da curva NURBS](../images/5-2/4/NURBScurve\_Degree.jpg)

> 1. Grau = 1
> 2. Grau = 2
> 3. Grau = 3

{% hint style="info" %}
Observe que quanto maior for o valor do grau, mais pontos de controle serão usados para interpolar a curva resultante.
{% endhint %}
