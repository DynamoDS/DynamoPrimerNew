# Aprofundar o conhecimento sobre o nó Sem toque

Com um entendimento de como criar um projeto Sem toque, podemos nos aprofundar nos detalhes específicos da criação de um nó ao navegar pelo exemplo ZeroTouchEssentials no Dynamo Github.

![Nós Sem toque](images/ootbzerotouch.png)

> Muitos dos nós padrão do Dynamo são essencialmente nós Sem toque, como a maioria dos nós matemáticos, de cor e de data e hora acima.

Para iniciar, faça o download do projeto ZeroTouchEssentials aqui: [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials)

No Visual Studio, abra o arquivo de solução `ZeroTouchEssentials.sln` e compile a solução.

![ZeroTouchEssentials no Visual Studio](images/vs-build-zte.jpg)

> O arquivo `ZeroTouchEssentials.cs` contém todos os métodos que iremos importar para o Dynamo.

Abra o Dynamo e importe o `ZeroTouchEssentials.dll` para obter os nós que faremos referência nos exemplos a seguir.

Os exemplos de código são extraídos e geralmente coincidem com [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs). A documentação XML foi removida para mantê-los concisos, e cada exemplo de código criará o nó na imagem acima dele.

### Valores de entrada padrão <a href="#default-input-values" id="default-input-values"></a>

O Dynamo suporta a definição de valores padrão para portas de entrada em um nó. Esses valores padrão serão fornecidos ao nó se as portas não tiverem conexões. Os padrões são expressos usando o mecanismo C# de especificação de argumentos opcionais no [Guia de programação C#](https://msdn.microsoft.com/pt-br/library/dd264739.aspx). Os valores padrão são especificados da seguinte maneira:

* Defina os parâmetros do método para um valor padrão: `inputNumber = 2.0`

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Valor padrão](images/defaultval.jpg)

> 1. O valor padrão será exibido quando você passar o cursor sobre a porta de entrada do nó

### Retornar vários valores <a href="#returning-multiple-values" id="returning-multiple-values"></a>

Retornar vários valores é um pouco mais complexo do que criar várias entradas e isso precisará ser feito usando um dicionário. As entradas do dicionário se tornam portas no lado de saída do nó. Várias portas de retorno são criadas da seguinte maneira:

* Adicione `using System.Collections.Generic;` para usar `Dictionary<>`.
* Adicione `using Autodesk.DesignScript.Runtime;` para usar o atributo `MultiReturn`. Isso faz referência ao “DynamoServices.dll” do pacote DynamoServices NuGet.
* Adicione o atributo `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` ao método. As sequências de caracteres se referem às chaves no dicionário e se tornarão os nomes das portas de saída.
* Retorne um `Dictionary<>` da função com chaves que coincidem com os nomes de parâmetro no atributo: `return new Dictionary<string, object>`

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> Consulte este exemplo de código em [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70)

Um nó que retorna várias saídas.

![Várias saídas](images/multipleoutputs.png)

> 1. Observe que agora há duas portas de saída nomeadas de acordo com as sequências que inserimos para as chaves do dicionário.

### Documentação, dicas de ferramentas e pesquisa <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

É recomendável adicionar documentação aos nós do Dynamo que descrevam a função, as entradas, as saídas, os identificadores de pesquisa do nó etc. Isso é feito por meio de identificadores de documentação XML. A documentação XML é criada da seguinte maneira:

* Qualquer texto de comentário que é precedido por três barras inclinadas adiante é considerado como documentação
  * Por exemplo: `/// Documentation text and XML goes here`
* Após as três barras, crie identificadores XML acima dos métodos que o Dynamo lerá ao importar o .dll
  * Por exemplo: `/// <summary>...</summary>`
* Ative a documentação XML no Visual Studio selecionando `Project > [Project] Properties > Build > Output` e marcando `Documentation file`

![Gerar um arquivo XML](images/vs-xml.jpg)

> 1. O Visual Studio gerará um arquivo XML na localização especificada

Os tipos de identificadores são os seguintes:

* `/// <summary>...</summary>` é a documentação principal do nó e será exibida como uma dica de ferramenta sobre o nó na barra lateral esquerda de pesquisa
* `/// <param name="inputName">...</param>` criará a documentação para os parâmetros de entrada específicos
* `/// <returns>...</returns>` criará a documentação para um parâmetro de saída
* `/// <returns name = "outputName">...</returns>` criará a documentação para vários parâmetros de saída
* `/// <search>...</search>` corresponderá o nó aos resultados da pesquisa com base em uma lista separada por vírgulas. Por exemplo, se criarmos um nó que subdivide uma malha, poderemos desejar adicionar identificadores como “malha”, “subdivisão” e “catmull-Clark”.

A seguir está um nó de exemplo com descrições de entrada e saída, bem como um resumo que será exibido na biblioteca.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Consulte este exemplo de código em [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80)

Observe que o código para esse nó de exemplo contém:

> 1. Um resumo dos nós
> 2. Uma descrição de entrada
> 3. Uma descrição de saída

#### Práticas recomendadas para as descrições dos nós do Dynamo 

As descrições dos nós descrevem brevemente a função e a saída de um nó. No Dynamo, elas aparecem em dois locais:

- Na dica de ferramenta do nó
- No navegador de documentação

![Descrição do nó](images/node-description.png)

Siga estas diretrizes para garantir a consistência e ajudar a economizar tempo ao escrever ou atualizar as descrições dos nós.

##### Visão geral

As descrições devem ter uma a duas frases. Se for necessário incluir mais informações, inclua-as no navegador de documentação na opção Em profundidade.

Maiúsculas e minúsculas da frase (coloque em maiúsculas a primeira palavra de uma frase e os nomes próprios). Sem ponto final.

A linguagem deve ser o mais clara e simples possível. Defina os acrônimos na primeira vez que forem mencionadas, a menos que sejam conhecidos até mesmo por usuários não especialistas.

Sempre priorize a clareza, mesmo que isso signifique desviar-se dessas diretrizes.

##### Diretrizes

| O que fazer      | O que não fazer |
| ----------- | ----------- |
| Inicie a descrição com um verbo em terceira pessoa. <ul><li>Exemplo: *Determina* se um objeto de geometria faz intersecção com outro</li></ul>      | Não comece com um verbo no infinitivo nem com substantivos. <ul><li>Exemplo: *Determinar* se um objeto de geometria faz intersecção com outro</li></ul>       |
| Use “Retorna”, “Cria” ou outro verbo descritivo em vez de “Obtém”. <ul><li>Exemplo: *Retorna* uma representação Nurbs de uma superfície</li></ul>   | Não use “Obter” ou “Obtém”. É menos específico e tem várias possíveis traduções. <ul><li>Exemplo: *Obtém* uma representação Nurbs da superfície</li></ul>        |
| Ao se referir a entradas, use “fornecido(a)” ou “entrada” em vez de “especificado(a)” ou outros termos. Omita “fornecido(a)” ou “entrada” quando possível para simplificar a descrição e reduzir a contagem de palavras. <ul><li>Exemplo: Exclui o arquivo *fornecido*</li><li>Exemplo: Projeta uma curva ao longo da direção de projeção *fornecida* na geometria base *fornecida*</li></ul>Você pode usar “especificado(a)” quando não se referir diretamente a uma entrada. <ul><li>Exemplo: Grava o conteúdo do texto em um arquivo *especificado* pelo caminho fornecido</li></ul>       | Ao se referir a entradas, para garantir a consistência, não use “especificado(a)” ou qualquer outro termo, exceto “fornecido(a)” ou “entrada”. Não misture “fornecido(a)” e “entrada” na mesma descrição, a menos que seja necessário para clareza. <ul><li>Exemplo: Exclui o arquivo *especificado*</li><li>Exemplo: Projeta uma curva de *entrada* ao longo de uma direção de projeção *fornecida* em uma geometria base *especificada*</li></ul>      |
| Use “um” ou “uma” ao se referir pela primeira vez a uma entrada. Use “fornecido(a)” ou “a entrada” em vez de “um” ou “uma”, conforme necessário para maior clareza.<ul><li>Exemplo: Faz varreduras em *uma* curva ao longo da curva do caminho</li></ul>      | Não use “este(a)” ou “neste(a)”ao se referir a uma entrada pela primeira vez. <ul><li>Exemplo: Faz varredura *nesta* curva ao longo da curva do caminho      |
| Ao se referir pela primeira vez a uma saída ou outro substantivo que seja o alvo da operação do nó, use “um” ou “uma”. Use apenas “o(a)” ao combinar com “entrada” ou “fornecido(a)”. <ul><li>Exemplo: Copia *um* arquivo</li><li>Exemplo: Copia *o arquivo fornecido*</li></ul>      | Ao se referir pela primeira vez a uma saída ou outro substantivo que é o alvo da operação do nó, não use “o(a)” sozinho. <ul><li>Exemplo: Copia *o* arquivo</li></ul>      |
| Coloque a primeira palavra de uma frase em maiúsculas e os nomes próprios, como nomes e substantivos tradicionalmente em maiúsculas. <ul><li>Exemplo: Retorna a intersecção de duas *BoundingBoxes*</li></ul>      | Não coloque objetos e conceitos de geometria comuns em maiúsculas, a menos que seja necessário para maior clareza. <ul><li>Exemplo: Dimensiona de forma não uniforme em torno do *plano* fornecido      |
| Use maiúsculas em Booleano. Use maiúsculas em True e False ao se referir à saída de Booleanos. <ul><li>Exemplo: Retornará *True* se os dois valores forem diferentes</li><li>Exemplo: Converte uma sequência de caracteres em todos os caracteres maiúsculos ou minúsculos com base em um parâmetro *Booleano*      | Não coloque Booleano em minúsculas. Não coloque True e False em minúsculas ao se referir à saída de Booleanos. <ul><li>Exemplo: Retornará *true* se os dois valores forem diferentes</li><li>Exemplo: Converte uma sequência de caracteres em todos os caracteres maiúsculos ou todos os caracteres minúsculos com base em um parâmetro *Booleano*</li></ul>

#### Avisos e erros dos nós do Dynamo

Os avisos e erros dos nós alertam o usuário sobre uma problema com o gráfico. Eles notificam o usuário sobre problemas que interferem na operação normal do gráfico exibindo um ícone e um balão de texto expandido acima do nó. Os erros e os avisos dos nós podem variar em gravidade: alguns gráficos podem ser executados adequadamente com avisos, enquanto outros bloqueiam os resultados esperados. Em todos os casos, os erros e os avisos dos nós são ferramentas importantes para manter o usuário atualizado sobre problemas com seu gráfico.

Para obter diretrizes para garantir consistência e ajudar a economizar tempo ao escrever ou atualizar mensagens de aviso e erro dos nós, consulte a página wiki [Padrão de conteúdo: avisos e erros dos nós](https://github.com/DynamoDS/Dynamo/wiki/Content-Pattern:-Node-Warnings-and-Errors).

### Objetos <a href="#objects" id="objects"></a>

O Dynamo não tem uma palavra-chave `new`, portanto, os objetos precisarão ser construídos usando métodos de construção estáticos. Os objetos são construídos da seguinte maneira:

* Tornar o construtor interno `internal ZeroTouchEssentials()`, a menos que seja necessário de outra forma
* Construir o objeto com um método estático como `public static ZeroTouchEssentials ByTwoDoubles(a, b)`

> Observação: O Dynamo usa o prefixo “Por” para indicar que um método estático é um construtor e, embora seja opcional, usar “Por” ajudará a biblioteca a se ajustar melhor ao estilo existente do Dynamo.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> Consulte este exemplo de código em [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26)

Após a importação do dll ZeroTouchEssentials, haverá um nó ZeroTouchEssentials na biblioteca. É possível criar esse objeto usando o nó `ByTwoDoubles`.

![Nó ByTwoDoubles](images/dyn-constructor.jpg)

### Usar tipos de geometria do Dynamo <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

As bibliotecas do Dynamo podem usar tipos de geometria nativos do Dynamo como entradas e criar uma nova geometria como saídas. Os tipos de geometria são criados da seguinte maneira:

* Referenciar “ProtoGeometry.dll” no projeto incluindo `using Autodesk.DesignScript.Geometry;` na parte superior do arquivo C# e adicionando o pacote NuGet da ZeroTouchLibrary ao projeto.
* **Importante:** Gerencie os recursos de geometria que não são retornados das funções. Consulte a seção **Dispor/usar declarações** abaixo.

> Observação: Os objetos de geometria do Dynamo são usados como qualquer outro objeto passado para funções.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Consulte este exemplo de código em [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86)

Um nó que obtém o comprimento de uma curva e o duplica.

![Entrada de curva](images/doublelength.png)

> 1. Esse nó aceita um tipo de geometria de curva como uma entrada.

### Dispor/usar declarações <a href="#disposeusing-statements" id="disposeusing-statements"></a>

Os recursos de geometria que não são retornados de funções precisarão ser gerenciados manualmente, a não ser que você esteja usando o Dynamo versão 2.5 ou posterior. No Dynamo 2.5 e versões posteriores, os recursos de geometria são manipulados internamente pelo sistema. No entanto, talvez você ainda precise descartar a geometria manualmente se tiver um caso de uso complexo ou precisar reduzir a memória em um momento determinado. O mecanismo do Dynamo lidará com quaisquer recursos de geometria que são retornados de funções. Os recursos de geometria que não são retornados podem ser manipulados manualmente das seguintes maneiras:

*   Com uma declaração de uso:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > A declaração de uso está documentada [aqui](https://msdn.microsoft.com/pt-br/library/yh598w02.aspx)
    >
    > Consulte [Melhorias na estabilidade da geometria do Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297) para saber mais sobre os novos recursos de estabilidade introduzidos no Dynamo 2.5
*   Com chamadas Dispor manuais:

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

### Migrações <a href="#migrations" id="migrations"></a>

Ao publicar uma versão mais recente de uma biblioteca, os nomes de nós podem ser alterados. As alterações de nome podem ser especificadas em um arquivo de migrações para que os gráficos criados em versões anteriores de uma biblioteca continuem a funcionar corretamente quando uma atualização é feita. As migrações são implementadas da seguinte maneira:

* Crie um arquivo `.xml` na mesma pasta que o `.dll` com o seguinte formato: “BaseDLLName”.Migrations.xml
* Em `.xml`, crie um único elemento `<migrations>...</migrations>`
* Dentro do elemento de migrações, crie os elementos `<priorNameHint>...</priorNameHint>` para cada alteração de nome
* Para cada alteração de nome, forneça um elemento `<oldName>...</oldName>` e `<newName>...</newName>`

![Arquivo de migração](images/vs-migrations-file.jpg)

> 1. Clicar com o botão direito do mouse e selecionar `Add > New Item`
> 2. Escolher `XML File`
> 3. Para este projeto, vamos nomear o arquivo de migrações `ZeroTouchEssentials.Migrations.xml`

Esse código de exemplo está informando ao Dynamo que qualquer nó denominado `GetClosestPoint` agora é nomeado `ClosestPointTo`.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> Consulte este exemplo de código em [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml)

### Elementos genéricos <a href="#generics" id="generics"></a>

No momento, o recurso Sem toque não suporta o uso de elementos genéricos. Eles podem ser usados, mas não no código que é importado diretamente onde o tipo não está definido. Métodos, propriedades ou classes que são genéricos e não têm o conjunto de tipos não podem ser expostos.

No exemplo abaixo, um nó Sem toque do tipo `T` não será importado. Se o resto da biblioteca for importado para o Dynamo, haverá exceções de tipo ausentes.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

O uso de um tipo genérico com o tipo definido neste exemplo será importado para o Dynamo.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
