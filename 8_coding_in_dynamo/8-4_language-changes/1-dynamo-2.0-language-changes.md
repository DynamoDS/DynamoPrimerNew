# Alterações de linguagem

A seção Alterações de linguagem fornece uma visão geral das atualizações e modificações feitas na linguagem no Dynamo em cada versão. Essas alterações podem afetar a funcionalidade, o desempenho e o uso, e este guia ajudará os usuários a entender quando e por que se adaptar a essas atualizações.

## Alterações de linguagem do Dynamo 2.0

1. Alteração de sintaxe de list@level de “@-1” para “@L1”
* Nova sintaxe para list@level, para usar list@L1 em vez de list@-1
* Motivação: alinhamento da sintaxe do código com a visualização/interface do usuário. Testes com o usuário mostram que essa nova sintaxe é mais compreensível

2. Implementação dos tipos Inteiro e Duplo no TS para alinhar com os tipos do Dynamo

3. Não permite funções sobrecarregadas nas quais os argumentos diferem somente por cardinalidade
* Os gráficos antigos que usam sobrecargas que foram removidas devem assumir como padrão as sobrecargas de classificação mais altas.
* Motivação: remover ambiguidade sobre qual função específica está sendo executada

4. Desativação da promoção de matriz com guias de replicação

5. As variáveis em blocos imperativos se tornaram locais no escopo do bloco imperativo
* Os valores de variável definidos dentro de blocos de código imperativos não serão alterados por alterações dentro de blocos imperativos que os referenciam.  

6. As variáveis se tornaram imutáveis para desativar a atualização associativa em nós de bloco de código

7. Compilação de todos os nós da interface do usuário para métodos estáticos

8. Declarações de devolução de suporte sem atribuição
* “=” não é necessário em definições de função ou código imperativo.

9. Migração de nomes antigos de métodos em CBNs
* Muitos nós foram renomeados para aumentar a legibilidade e a colocação na interface do usuário do navegador de biblioteca

10. Listagem como limpeza de dicionário

----
Problemas conhecidos:
- Os conflitos de namespace em blocos imperativos causam o aparecimento de portas de entrada inesperadas. Consulte o [problema do Github](https://github.com/DynamoDS/Dynamo/issues/8796) para obter mais informações. Para resolver isso, defina a função fora do Bloco imperativo assim:
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Alterações da linguagem do Dynamo 2.0 explicadas

Várias melhorias foram feitas na linguagem na versão do Dynamo 2.0. As principais motivações para isso foram simplificar a linguagem. A ênfase tem sido em tornar o DesignScript mais compreensível e simples de usar em favor de torná-lo mais poderoso e flexível com o objetivo de melhorar a capacidade de compreensão do usuário final.

A seguir está a lista de alterações na versão 2.0 explicadas:
* Sintaxe de List@Level simplificada
* Os métodos sobrecarregados com parâmetros que se diferenciam apenas por classificação são inválidos 
* Compilação de todos os nós da interface do usuário como métodos estáticos
* A promoção de lista foi desativada quando usada com guias de replicação/amarras
* As variáveis em blocos associativos são imutáveis para impedir a atualização associativa
* As variáveis em blocos imperativos são locais para o escopo imperativo
* Separação de listas e dicionários

## 1\. Sintaxe de list@level simplificada 

Nova sintaxe para list@level, para usar `list@L1` em vez de `list@-1` ![](../images/8-4/1/lang2_1.png)


## 2\. As funções sobrecarregadas com parâmetros que se diferenciam apenas por classificação são inválidas
As funções sobrecarregadas são problemáticas por uma série de razões:
* Uma função sobrecarregada indicada por um nó de interface do usuário no gráfico pode não ser a mesma sobrecarga que é executada no tempo de execução
* A resolução do método é cara e não funciona bem para funções sobrecarregadas
* É difícil entender o comportamento de replicação para funções sobrecarregadas

Tomemos `BoundingBox.ByGeometry` como exemplo (havia duas funções sobrecarregadas em versões mais antigas do Dynamo), uma que considerava um único argumento de valor e a outra que tomava uma lista de geometrias como argumento:
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Se o usuário soltar o primeiro nó na tela e conectar uma lista de geometrias, ele esperará que a replicação entre em ação, mas isso nunca acontecerá, porque no tempo de execução a segunda sobrecarga será chamada em vez disso, conforme mostrado: ![](../images/8-4/1/lang2_2.png)
 
Na versão 2.0, não permitimos funções sobrecarregadas que só diferem na cardinalidade do parâmetro por esse motivo. Isso significa que para funções sobrecarregadas que têm o mesmo número e tipos de parâmetros, mas têm um ou mais parâmetros que diferem apenas na classificação, a sobrecarga que é definida primeiro sempre vence enquanto o resto é descartado pelo compilador. A principal vantagem de fazer essa simplificação é simplificar a lógica de resolução do método, tendo um caminho rápido para selecionar candidatos à função.

Na biblioteca de geometria da versão 2.0, a primeira sobrecarga no exemplo `BoundingBox.ByGeometry` se tornou obsoleta e a segunda foi mantida; portanto, se o nó for destinado à replicação, ou seja, usado no contexto do primeiro, ele precisará ser usado com a opção de amarra mais curta (ou mais longa) ou em um bloco de código com guias de replicação: 
```
BoundingBox.ByGeometry(geometry<1>);
```
Podemos ver neste exemplo que o nó de classificação mais alta pode ser usado tanto em uma chamada replicada quanto em uma não replicada e, portanto, é sempre preferível a uma sobrecarga de classificação inferior. Como regra geral, portanto, os **autores dos nós são sempre aconselhados a eliminar as sobrecargas de classificação mais baixas em favor de métodos de classificação mais alta** para que o compilador DesignScript sempre chame o método de classificação mais alta como o primeiro e único que ele encontra.

### Exemplos:
No exemplo a seguir, duas sobrecargas de função `foo` foram definidas. Na versão 1.x, é ambíguo qual sobrecarga é executada no tempo de execução. O usuário pode esperar que o segundo `foo(a:int, b:int)` de sobrecarga seja executado, caso em que o método deverá replicar três vezes retornando um valor de `10` três vezes. Na realidade, o que é retornado é um único valor de `10` quando a primeira sobrecarga com o parâmetro de lista é chamada.

### A segunda sobrecarga é omitida na versão 2.0:
Na versão 2.0, é sempre o primeiro método definido que é escolhido sobre o resto. Prevalência por ordem de chegada.

![](../images/8-4/1/lang2_3.png)

Para cada um dos casos a seguir, a primeira sobrecarga definida será considerada. Observe que ela é puramente baseada na ordem de definição das funções e não nas classificações de parâmetros, embora seja recomendável dar preferência a métodos com parâmetros de classificação mais altos para nós Sem toque e definidos pelo usuário.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Compilação de todos os nós da interface do usuário para métodos estáticos
No Dynamo 1.x, os nós da interface do usuário (blocos sem código) foram compilados para métodos de instância e propriedades, respectivamente. Por exemplo, o nó `Point.X` foi compilado para `pt.X` e `Curve.PointAtParameter` foi compilado para `curve.PointAtParameter(param)`. Esse comportamento apresentava dois problemas:

__A. A função que o nó da interface do usuário representava nem sempre era a mesma função executada no tempo de execução__

Um exemplo típico é o nó `Translate`. Há vários nós `Translate` que têm o mesmo número e tipos de argumentos como: `Geometry.Translate`, `Mesh.Translate` e `FamilyInstance.Translate`. Devido ao fato de que os nós eram compilados como métodos de instância, a transmissão de um `FamilyInstance` para um nó `Geometry.Translate` surpreendentemente ainda funcionava, pois no tempo de execução ele enviava a chamada para o método de instância `Translate` em um `FamilyInstance`. Isso era obviamente enganoso para os usuários, pois o nó não fazia o que dizia.

__B. O segundo problema era que os métodos de instância não funcionavam com matrizes heterogêneas__

No tempo de execução, o mecanismo de execução precisa descobrir para qual função deve ser enviado. Se a entrada for uma lista, digamos `list.Translate()`, como é caro passar por cada elemento em uma lista e métodos de pesquisa em seu tipo, a lógica de resolução de método simplesmente assumia que o tipo de destino era o mesmo que o tipo do primeiro elemento e tentava procurar o método `Translate()` definido nesse tipo. Como resultado, se o primeiro tipo de elemento não correspondesse ao tipo de destino do método (ou mesmo que fosse `null` ou uma lista vazia), ocorria uma falha em toda a lista mesmo que houvesse outros tipos na lista que coincidissem. 

Por exemplo, se uma entrada de lista com os seguintes tipos `[Arc, Line]` fosse passada para `Arc.CenterPoint`, o resultado continha um ponto central para o arco e um valor de `null` para a linha, conforme esperado. No entanto, se a ordem fosse invertida, todo o resultado era nulo, pois ocorria uma falha no primeiro elemento na verificação de resolução do método:
### Dynamo 1.x: testa somente o primeiro elemento da lista de entrada para verificação da resolução do método
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
Na versão 2.0, esses dois problemas são resolvidos compilando os nós da interface do usuário como propriedades estáticas e métodos estáticos. 

Com métodos estáticos, a resolução do método de tempo de execução é mais simples e todos os elementos na lista de entrada são iterados. Por exemplo:

A semântica de `foo.Bar()` (método de instância) precisa verificar o tipo de `foo` e também verificar se é uma lista ou não e, em seguida, combiná-la com as funções candidatas. Isso é caro. Por outro lado, a semântica de `Foo.Bar(foo)` (método estático) só precisa verificar uma função com o tipo de parâmetro `foo`.

Isto é o que acontece na versão 2.0:
* Um nó de propriedade da interface do usuário é compilado em um getter estático: o mecanismo gera uma versão estática de um getter para cada propriedade. Por exemplo, um nó `Point.X` é compilado em um getter estático `Point.get_X(pt)`. Observe que o getter estático também pode ser chamado usando seu alias: `Point.X(pt)` em um nó de bloco de código.
* Um nó do método da interface do usuário é compilado para a versão estática: o mecanismo gera um método estático correspondente para o nó. Por exemplo, o nó `Curve.PointAtParameter` é compilado para `Curve.PointAtParameter(curve: Curve, parameter:double)` em vez de `curve.PointAtParameter(parameter)`. 

**Observação:** Não removemos o suporte ao método de instância com essa alteração; portanto, os métodos de instância existentes usados em CBNs, como `pt.X` e `curve.PointAtParameter(parameter)` nos exemplos acima, ainda funcionarão.

Esse exemplo funcionava anteriormente em 1.x, pois o gráfico compilava para `point.X;` e encontrava a propriedade `X` no objeto de ponto. Agora ocorre uma falha na versão 2.0 como o código compilado – `Vector.X(point)` espera apenas um tipo `Vector`:

![](../images/8-4/1/lang2_5.png)

### Vantagens:
**Coerente/compreensível:** os métodos estáticos eliminam qualquer ambiguidade sobre qual método será executado no tempo de execução. O método sempre coincide com o nó da interface do usuário usado no gráfico que o usuário espera que seja chamado.

**Compatível:** há melhor correlação entre o código e o programa visual.

**Instrucional:** a transmissão de entradas de lista heterogêneas para nós agora resulta em valores não nulos para tipos que são aceitos pelo nó e valores nulos para tipos que não implementam o nó. Os resultados são mais previsíveis e indicam melhor quais são os tipos permitidos para o nó.

### Advertência: Ambiguidades não resolvidas com métodos sobrecarregados

Como o Dynamo aceita sobrecargas de função em geral, ainda poderá haver confusão se houver outra função sobrecarregada com o mesmo número de parâmetros. Por exemplo, no gráfico a seguir, se conectarmos um valor numérico à entrada `direction` de `Curve.Extrude` e um vetor à entrada `distance` de `Curve.Extrude`, ambos os nós continuarão a funcionar, o que é inesperado. Nesse caso, mesmo que os nós compilem para métodos estáticos, o mecanismo ainda será incapaz de perceber a diferença no tempo de execução e escolhe qualquer um dependendo do tipo de entrada. ![](../images/8-4/1/lang2_6.png)
 
### Problemas solucionados:
A mudança para a semântica do método estático introduziu os seguintes efeitos colaterais que valem a pena mencionar aqui como mudanças de linguagem da versão 2.0 relacionadas.

**1\. Perda de comportamento polimórfico:**

Vamos considerar um exemplo de nós `TSpline` em `ProtoGeometry` (observe que `TSplineTopology` herda do tipo de `Topology` base): o nó `Topology.Edges` que foi compilado anteriormente para o método de instância, `object.Edges`, agora é compilado para o método estático, `Topology.Edges(object)`. A chamada anterior resolveria polimorficamente para o método de classe derivada, `TsplineTopology.Edges` após um envio de método sobre o tipo de tempo de execução do objeto. 

![](../images/8-4/1/lang2_7.png)

Considerando que o novo comportamento estático foi forçado a chamar o método de classe base, `Topology.Edges`. Como resultado, esse nó retornou a classe base, os objetos `Edge` em vez dos objetos de classe derivados do tipo `TSplineEdge`.
 
![](../images/8-4/1/lang2_8.png)

Essa foi uma regressão à medida que os nós de `TSpline` a jusante que esperavam o começo da falha do `TSplineEdges`. 

O problema foi corrigido ao adicionar uma verificação de tempo de execução na lógica de envio do método para verificar o tipo de instância em relação ao tipo ou um subtipo do primeiro parâmetro do método. No caso de uma lista de entrada, simplificamos o envio de método para simplesmente verificar o tipo do primeiro elemento. Assim, a solução final foi uma acomodação entre uma pesquisa de método em parte estática e em parte dinâmica.

**Novo comportamento polimórfico na versão 2.0:**

![](../images/8-4/1/lang2_9.png)

Neste caso, como o primeiro elemento `a` é um `TSpline`, é o método derivado `TSplineTopology.Edges` que é chamado no tempo de execução. Como resultado, retorna `null` para o tipo de `Topology` `b` base. 

No segundo caso, como o tipo de `Topology` `b` geral é o primeiro elemento, o método de `Topology.Edges` base é chamado. Como `Topology.Edges` também aceita o tipo de `TSplineTopology` derivado, `a` como entrada, ele retorna `Edges` para ambas as entradas, `a` e `b`.

![](../images/8-4/1/lang2_10.png)
 
**2\. Regressões na produção de listas externas redundantes**

Há uma diferença principal entre os métodos de instância e os métodos estáticos quando se trata do comportamento do guia de replicação. Com os métodos de instância, as entradas de valor único com guias de replicação não são promovidas para listas, enquanto são promovidas para métodos estáticos.

Considere o exemplo de nó `Surface.PointAtParameter` entre amarras e com uma única entrada de superfície e matrizes de valores de parâmetro `u` e `v`. O método de instância é compilado para:
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
resultando em uma matriz 2D de pontos.
 
O método estático é compilado para:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
resultando em uma lista 3D de pontos com uma lista redundante externa.

Esse efeito colateral de compilar nós da interface do usuário para métodos estáticos podia potencialmente causar regressões nesses casos de uso existentes. Esse problema foi resolvido desativando-se a promoção de entradas de valor único em uma lista quando usada com guias de replicação/amarras (consulte o próximo item).
 
**4\. Promoção de listas desativada com guias de replicação/amarras**

Na versão 1.x, havia dois casos em que os valores únicos eram promovidos a listas:

* Quando as entradas de classificação inferior eram passadas para funções que esperavam entradas de classificação mais alta
* Quando as entradas de classificação inferior eram passadas para funções que esperam a mesma classificação, mas nas quais os argumentos de entrada são decorados com guias de replicação ou usam amarras

Na versão 2.0, não oferecemos mais suporte ao último caso, impedindo a promoção de listas nesses cenários.

No gráfico 1.x a seguir, um nível de guia de replicação para cada promoção de matriz imposta `y` e `z` de classificação 1 para cada um deles, razão pela qual o resultado tinha uma classificação de 3 (1 cada para `x`, `y` e `z`). Em vez disso, um usuário esperava que o resultado fosse de classificação 1, já que não é óbvio que a presença de guias de replicação para entradas de valor único adicionava níveis ao resultado.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dynamo 1.x: lista 3D de pontos

![](../images/8-4/1/lang2_11.png)

Na versão 2.0, a presença de guias de replicação para cada um dos argumentos de valor único `y` e `z` não causa uma promoção, resultando em uma lista com a mesma cota que a lista 1D de entrada para `x`. 

### Dynamo 2.0: lista 1D de pontos

![](../images/8-4/1/lang2_12.png)

A regressão acima mencionada causada pela compilação do método estático com a geração de listas externas redundantes também foi abordada por essa alteração de linguagem.

Continuando com o mesmo exemplo acima, vimos que uma chamada de método estático como:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
produziu uma lista 3D de pontos no Dynamo 1.x. Isso aconteceu devido à promoção da primeira superfície de argumento de valor único a uma lista quando usada com um guia de replicação.
 
### Dynamo 1.x: promoção de lista de argumento com guia de replicação

![](../images/8-4/1/lang2_13.png)

Na versão 2.0, desativamos a promoção de argumentos de valor único a listas quando usadas com guias de replicação ou amarras. Então agora a chamada para:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
simplesmente retorna uma lista 2D pois a superfície não é promovida.

### Dynamo 2.0: promoção de lista de argumento de valor único com guia de replicação desativada

![](../images/8-4/1/lang2_14.png)

Essa alteração agora remove a adição de um nível de lista redundante e também resolve a regressão causada pela transição para a compilação de método estático.

### Vantagens:

**Legível:** os resultados se alinham com as expectativas do usuário e são mais fáceis de compreender

**Compatível:** nós de interface do usuário (com opção de amarra) e CBNs que usam guias de replicação fornecem resultados compatíveis

**Consistente:** 
* Os métodos de instância e os métodos estáticos são consistentes (corrigem problemas com a semântica do método estático)
* Os nós com entradas e com argumentos padrão se comportam de forma consistente (veja abaixo)

![](../images/8-4/1/lang2_15.png)

## 5\. As variáveis são imutáveis nos nós de bloco de código para impedir a atualização associativa 

O DesignScript historicamente permite dois paradigmas de programação – programação associativa e imperativa. O código associativo cria um gráfico de dependência com base nas instruções de programa em que as variáveis têm uma dependência entre si. A atualização de uma variável pode acionar atualizações para todas as outras variáveis que dependem dessa variável. Isso significa que a sequência de execução de instruções em um bloco associativo não é baseada em sua ordem, mas nas relações de dependência entre variáveis.

No exemplo a seguir, a sequência de execução do código é linhas 1 -> 2 -> 3 -> 2. Como `b` tem uma dependência de `a`, quando `a` é atualizado na linha 3, a execução salta para a linha 2 novamente para atualizar `b` com o novo valor de `a`. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
Em contraste, se o mesmo código é executado em um contexto imperativo, as instruções são executadas em um fluxo linear de cima para baixo. Os blocos de código imperativos são, portanto, adequados para a execução sequencial de construções de código, como loops e condições if-else.

### Ambiguidades da atualização associativa:

**1\. Variáveis com dependência cíclica:**

Em certos casos, uma dependência cíclica entre variáveis pode não ser tão óbvia como no caso a seguir. Nesses casos, nos quais o compilador não pode detectar o ciclo estaticamente, isso pode levar a um ciclo de tempo de execução indefinido.
```
a = 1;
b = a;
a = b;
```
**2\. Variáveis que dependentes de si mesmas:**

Se uma variável depende de si mesma, seu valor deve se acumular ou deve ser redefinido para seu valor original a cada atualização?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
Neste exemplo de geometria, como o cubo `b` depende de si mesmo, bem como do cilindro, `a`, mover o controle deslizante deve fazer com que o furo se mova ao longo do bloco ou deve criar um efeito cumulativo de perfurar vários furos ao longo do caminho para cada atualização de posição do controle deslizante?

![](../images/8-4/1/lang2_16.gif)

**3\. Atualização de propriedades de variáveis:**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Atualização de funções:**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
Com base na experiência, descobrimos que a atualização associativa não se mostra útil em nós de bloco de código em um contexto de gráfico de fluxo de dados baseado em nós. Antes de qualquer ambiente de programação visual estar disponível, a única maneira de explorar opções era alterar explicitamente os valores de algumas variáveis no programa. Um programa baseado em texto tem o histórico completo de atualizações de uma variável, enquanto em um ambiente de programação visual, somente o último valor de uma variável é exibido. 

Se de alguma forma ele foi usado por alguns usuários, provavelmente foi sem saber, causando mais danos do que benefícios. Portanto, decidimos na versão 2.0 ocultar a associatividade no uso de nós do bloco de código, tornando as variáveis imutáveis enquanto continuamos a manter a atualização associativa apenas como um recurso nativo do mecanismo DS. Essa é outra mudança feita com a ideia de simplificar a experiência dos scripts para os usuários.

**A atualização associativa está desativada em CBNs impedindo a redefinição de variáveis:** ![](../images/8-4/1/lang2_17.png)

**A indexação de listas ainda é permitida em blocos de código**

Uma exceção foi feita para a indexação de listas e ainda é permitida na versão 2.0 com a atribuição de operador de índice.

Neste próximo exemplo, vemos que a lista `a` é inicializada, mas pode ser posteriormente substituída por uma atribuição de operador de índice e que quaisquer variáveis dependentes de `a` seriam atualizadas associativamente conforme visto pelo valor de `c`. Além disso, a visualização do nó exibe os valores atualizados de `a` após a redefinição de um ou mais de seus elementos.

![](../images/8-4/1/lang2_18.png)

## 6\. As variáveis em blocos imperativos são locais para o escopo do bloco imperativo

Fizemos alterações imperativas na regra de escopo para a versão 2.0 para desativar cenários complicados de atualização entre linguagens.

No Dynamo 1.x, a sequência de execução do script a seguir seria das linhas 1 -> 2 -> 4 -> 6 -> 4, na qual uma alteração é propagada do escopo da linguagem externa para a interna. Depois que `y` é atualizado no bloco associativo externo e como `x` no bloco imperativo tem uma dependência de `y`, o controle muda do programa associativo externo para a linguagem imperativa na linha 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

A sequência de execução neste próximo exemplo seria linhas: 1 -> 2 -> 4 -> 2, na qual a alteração se propagaria do escopo de linguagem interno para o externo.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Os cenários acima referem-se à atualização entre linguagens, que assim como a atualização associativa não são muito úteis em nós de bloco de código. Para desativar cenários complexos de atualização entre linguagens, tornamos as variáveis no escopo imperativo locais. 

No exemplo a seguir, no Dynamo 2.0:
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* `x` definido no bloco imperativo agora é local para o escopo imperativo
* Os valores de `x` e `y` no escopo externo permanecem `1` e `2`, respectivamente

Qualquer variável local dentro de um bloco imperativo precisará ser retornada se seu valor tiver que ser acessado em um escopo externo.

Considere o exemplo a seguir:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* `y` é copiado localmente dentro do escopo imperativo  
* O valor de `x` local para o escopo imperativo é `4`
* A atualização do valor de `y` no escopo externo continua a causar a atualização do `x` devido à atualização entre linguagens, mas está desativada em blocos de código na versão 2.0 devido à imutabilidade da variável
* Os valores de `x` e `y` no escopo associativo externo permanecem `1` e `2`, respectivamente

## 7\. Listas e dicionários

No Dynamo 1.x, as listas e os dicionários eram representados por um único contêiner unificado, que podia ser indexado tanto por um índice inteiro quanto por uma chave não integral. A tabela a seguir resume a separação entre listas e dicionários na versão 2.0 e as regras do novo tipo de dados de dicionário:

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Inicialização de lista**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Lista vazia**                | `a = {};`                   | `a = [];`                                |
| **Inicialização de dicionário** | **Podia anexar ao mesmo dicionário dinamicamente:** | **Só pode criar novos dicionários:** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Cria um dicionário vazio |
|                           | `a[“baz”] = 3;`             |                                          |
| **Indexação de dicionário**   | **Indexação de chave**            | **A sintaxe de indexação permanece a mesma**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Chaves de dicionário**       | **Qualquer tipo de chave era válida**  | **Somente chaves de sequência de caracteres são válidas**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Nova sintaxe de lista `[]`
A sintaxe de inicialização de lista foi alterada de chaves `{}` para colchetes `[]` na versão 2.0. Todos os scripts 1.x são migrados automaticamente para a nova sintaxe quando abertos na versão 2.0. 

**Observação sobre os atributos de argumento padrão nos nós Sem toque:**

No entanto, observe que a migração automática não funciona com a sintaxe antiga usada nos atributos de argumento padrão. Os autores de nós precisariam atualizar manualmente suas definições do método Sem toque para usar a nova sintaxe em atributos de argumento padrão `DefaultArgumentAttribute` se necessário.

**Observação sobre a indexação:**

O novo comportamento de indexação foi alterado em alguns casos. Agora, a indexação em uma lista ou um dicionário com uma lista arbitrária de índices/chaves usando o operador `[]` preserva a estrutura de lista da lista de entrada de índices/chaves. Anteriormente, ela sempre retornava uma lista 1D de valores:
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Sintaxe de inicialização do dicionário:

A sintaxe de `{}` (sintaxe de chaves) para a inicialização do dicionário somente pode ser usada no formato de par chave-valor 
```
dict = {<key> : <value>, …}; 
```
em que apenas uma sequência de caracteres é permitida para `<key>` e vários pares chave-valor são separados por vírgulas.

![](../images/8-4/1/lang2_19.png)

O método Sem toque `Dictionary.ByKeysValues` pode ser usado como uma maneira mais versátil de inicializar um dicionário, passando uma lista de chaves e valores, respectivamente, e tendo todos os recursos dos métodos Sem toque, como guias de replicação etc.

![](../images/8-4/1/lang2_20.png)

### Por que não usamos expressões arbitrárias para a sintaxe de inicialização do dicionário?

Testamos a ideia de usar expressões arbitrárias para chaves na sintaxe de inicialização de valor-chave do Dicionário e descobrimos que isso poderia levar a resultados confusos, especialmente quando uma sintaxe como `{keys : vals}` (`keys`, `vals` ambos representando listas) interferia com outros recursos de linguagem do DesignScript, como replicação, e produzia resultados diferentes com base no nó inicializador Sem toque. 

Por exemplo, pode haver outros casos como esta declaração em que seria difícil definir o comportamento esperado:
```
dict = {["foo", "bar"] : "baz" };
```
Adicionar mais sintaxe de guia de replicação etc. à mistura, não apenas identificadores, seria ir contra a ideia de simplicidade na linguagem. 

_Poderíamos_ estender chaves de dicionário para aceitar expressões arbitrárias no futuro, mas também teremos que garantir que a interação com outros recursos de linguagem seja consistente e inteligível ao custo de adicionar complexidade em vez de tornar o sistema um pouco menos poderoso, mas simples de entender. Considerando que há sempre uma maneira alternativa de resolver isso usando o método `Dictionary.ByKeysValues(keyList, valueList)`, o que não exigiria muito esforço.

### Interação com nós Sem toque:

__1\. O nó Sem toque que retorna um dicionário .NET é retornado como um dicionário do Dynamo__

**Considere o seguinte método C# Sem toque que retorna um IDictionary:** ![](../images/8-4/1/lang2_21.png)

**É realizado marshal do valor de retorno do nó ZT correspondente como um dicionário do Dynamo:** ![](../images/8-4/1/lang2_22.png)

__2\. Nós de múltiplos retornos são visualizados como dicionários__

**O nó Sem toque que retorna o IDictionary com atributo de múltiplos retornos retorna um dicionário do Dynamo:** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. O dicionário do Dynamo pode ser passado como entrada no nó Sem toque que aceita o dicionário .NET__

**Método ZT com um parâmetro do IDictionary:** ![](../images/8-4/1/lang2_25.png)

**O nó ZT aceita o dicionário do Dynamo como entrada:** ![](../images/8-4/1/lang2_26.png)

### Visualização de dicionário em nós de múltiplos retornos

Os dicionários são pares chave-valor não ordenados. Consistente com essa ideia, as visualizações do par chave-valor dos nós que retornam dicionários não são, portanto, organizadas de forma garantida na ordem dos valores de retorno dos nós. 

No entanto, abrimos uma exceção para nós com múltiplos retornos que tem`MultiReturnAttribute` definido. No exemplo a seguir, o nó `DateTime.Components` é um nó “de múltiplos retornos” e a visualização do nó reflete seus pares valor-chave para que fiquem na mesma ordem que a das portas de saída no nó, que também é a ordem em que as saídas são especificadas com base no`MultiReturnAttribute` na definição do nó.

Observe também que as visualizações dos blocos de código não são ordenadas, ao contrário do nó da interface do usuário, pois as informações da porta de saída (na forma de um atributo de retorno múltiplo) não existem para o nó do bloco de código: ![](../images/8-4/1/lang2_27.png)
