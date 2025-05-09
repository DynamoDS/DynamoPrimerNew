# Personalização avançada de nós do Dynamo

Com um conhecimento básico do nó Sem toque já estabelecido, esta seção se aprofunda nas vantagens de personalizar os nós do Dynamo para aprimorar a funcionalidade e a experiência do usuário. Ao adicionar recursos como mensagens de aviso, mensagens informativas e ícones personalizados, é possível criar nós mais intuitivos, informativos e visualmente envolventes. Além de ajudar os usuários a entender possíveis problemas ou otimizar seus fluxos de trabalho, essas personalizações também fazem com que seus nós se destaquem como ferramentas profissionais e fáceis de usar.

Personalizar nós é uma excelente maneira de garantir que suas soluções sejam claras, confiáveis e adaptadas para atender às necessidades específicas do projeto.

## Gerar mensagens de aviso personalizadas usando OnLogWarningMessage <a href="#generating-custom-warning-messages-using-onlogwarningmessage" id="generating-custom-warning-messages-using-onlogwarningmessage"></a>

No Dynamo, o método `OnLogWarningMessage` fornece uma maneira de registrar mensagens de aviso diretamente no console do Dynamo. Esse é um recurso poderoso, especialmente para os nós Sem toque, pois permite que os desenvolvedores alertem os usuários quando há problemas com entradas ou parâmetros que podem levar a um comportamento inesperado. Este guia ensinará como implementar `OnLogWarningMessage` em qualquer nó Sem Toque.

### Etapas de implementação de `OnLogWarningMessage` <a href="#implementation-step-for-onlogwarningmessage" id="implementation-step-for-onlogwarningmessage"></a>

#### Etapa 1: Importar o namespace necessário <a href="#import-the-required-namespace" id="import-the-required-namespace"></a>

`OnLogWarningMessage` faz parte do namespace `DynamoServices`, por isso, comece adicionando-o ao arquivo do projeto.

```
using DynamoServices;
```

#### Etapa 2: Identificar quando registrar avisos <a href="#identify-when-to-log-warnings" id="identify-when-to-log-warnings"></a>

Antes de adicionar uma mensagem de aviso, considere a lógica no método:

* Quais condições podem causar resultados incorretos ou inesperados?
* Há valores ou parâmetros de entrada específicos que o método requer para funcionar corretamente?

Exemplos de condições a serem verificadas:

* **Valores fora do intervalo** (por exemplo, `if (inputValue < 0)`).
* **Coleções nulas ou vazias** (por exemplo, `if (list == null || list.Count == 0)`).
* **O tipo de dados não coincide** (por exemplo, se um tipo de arquivo não for suportado).

#### Etapa 3: Usar o `OnLogWarningMessage` para registrar o aviso <a href="#use-onlogwarningmessage-to-log-the-warning" id="use-onlogwarningmessage-to-log-the-warning"></a>

Coloque chamadas `OnLogWarningMessage` onde são detectadas as condições que podem causar problemas. Quando a condição for atendida, registre uma mensagem de aviso que forneça orientação clara ao usuário.

### Sintaxe para `OnLogWarningMessage` <a href="#syntax-for-onlogwarningmessage" id="syntax-for-onlogwarningmessage"></a>

```
LogWarningMessageEvents.OnLogWarningMessage("Your warning message here.");
```

### Exemplo de implementações de `OnLogWarningMessage` <a href="#example-implementations-of-onlogwarningmessage" id="example-implementations-of-onlogwarningmessage"></a>

Para demonstrar `OnLogWarningMessage` na prática, aqui estão diferentes cenários que podem ser encontrados ao criar um nó Sem Toque.

#### Exemplo 1: Validar entradas numéricas <a href="#example-1-validating-numeric-inputs" id="example-1-validating-numeric-inputs"></a>

Neste exemplo, vamos nos basear no nó personalizado criado no **“Estudo de caso do nó Sem toque – Nó de grade”** anterior; um método denominado `RectangularGrid` que gera uma grade de retângulos com base em entradas `xCount` e `yCount`. Vamos percorrer o processo de teste Se uma entrada é inválida e, em seguida, usar `OnLogWarningMessage` para registrar um aviso e parar o processamento.

![Exemplo 1 de OnLogWarningMessage](images/onlogwarningmessage-example-1.png)

**Usar `OnLogWarningMessage` para validação de entrada**

Quando gerar uma grade com base em `xCount` e `yCount`. Recomenda-se garantir que os dois valores sejam inteiros positivos antes de continuar.

```
public static List<Rectangle> CreateGrid(int xCount, int yCount)
{
    // Check if xCount and yCount are positive
    if (xCount <= 0 || yCount <= 0)
    {
        LogWarningMessageEvents.OnLogWarningMessage("Grid count values must be positive integers.");
        return new List<Rectangle>();  // Return an empty list if inputs are invalid
    }
    // Proceed with grid creation...
}
```

Neste exemplo:

* **Condição**: se `xCount` ou `yCount` for menor ou igual a zero.
* **Mensagem**: `"Grid count values must be positive integers."`

O aviso será mostrado no Dynamo se um usuário inserir valores zero ou negativos, ajudando-os a entender a entrada esperada.

Agora que sabemos como é, podemos implementá-lo no nó de exemplo Grades:

```
using Autodesk.DesignScript.Geometry;
using DynamoServices;

namespace CustomNodes
{
    public class Grids
    {
        // The empty private constructor.
        // This will not be imported into Dynamo.
        private Grids() { }

        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            // Check for valid input values
            if (xCount <= 0 || yCount <= 0)
            {
                // Log a warning message if the input values are invalid
                LogWarningMessageEvents.OnLogWarningMessage("Grid count values must be positive integers.");
                return new List<Rectangle>(); // Return an empty list if inputs are invalid
            }

            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }

            return pList;
        }
    }
}
```

**Exemplo 2: verificar coleções nulas ou vazias**

Se o método exigir uma lista de pontos, mas um usuário passar uma lista vazia ou nula, será possível usar `OnLogWarningMessage` para informá-lo sobre o problema.

![Exemplo 2 de OnLogWarningMessage](images/onlogwarningmessage-example-2.png)

```
public static Polygon CreatePolygonFromPoints(List<Point> points)
{
    if (points == null || points.Count < 3)
    {
        LogWarningMessageEvents.OnLogWarningMessage("Point list cannot be null or have fewer than three points.");
        return null;  // Return null if the input list is invalid
    }
    // Proceed with polygon creation...
}
```

Neste exemplo:

* **Condição**: se a lista `points` for nula ou contiver menos de três pontos.
* **Mensagem**: `"Point list cannot be null or have fewer than three points."`

Avisa aos usuários que eles precisam passar por uma lista válida com pelo menos três pontos para formar um polígono.

***

**Exemplo 3: verificar a compatibilidade de tipo de arquivo**

Para um nó que processa caminhos de arquivo, pode ser necessário garantir que apenas determinados tipos de arquivo sejam permitidos. Se um tipo de arquivo sem suporte for detectado, registre um aviso.

![Exemplo 3 de OnLogWarningMessage](images/onlogwarningmessage-example-3.png)

```
public static void ProcessFile(string filePath)
{
    if (!filePath.EndsWith(".csv"))
    {
        LogWarningMessageEvents.OnLogWarningMessage("Only CSV files are supported.");
        return;
    }
    // Proceed with file processing...
}
```

Neste exemplo:

* **Condição**: se o caminho do arquivo não terminar com “.csv”.
* **Mensagem**: `"Only CSV files are supported."`

Avisa os usuários para garantir que estão transmitindo um arquivo CSV, ajudando a evitar problemas relacionados a formatos de arquivo incompatíveis.

## Adicionar mensagens informativas com `OnLogInfoMessage` <a href="#adding-informational-messages-with-onloginfomessage" id="adding-informational-messages-with-onloginfomessage"></a>

No Dynamo, o `OnLogInfoMessage` do namespace `DynamoServices` permite que os desenvolvedores registrem mensagens informativas diretamente no console do Dynamo. Isso é útil para confirmar operações bem-sucedidas, comunicar o progresso ou fornecer informações adicionais sobre as ações do nó. Este guia ensinará como adicionar `OnLogInfoMessage` em qualquer nó Sem Toque para aprimorar o feedback e a experiência do usuário.

### Etapas de implementação de `OnLogInfoMessage` <a href="#implementation-steps-for-onloginfomessage" id="implementation-steps-for-onloginfomessage"></a>

#### Etapa 1: Importar o namespace necessário <a href="#step-1-import-the-required-namespace" id="step-1-import-the-required-namespace"></a>

`OnLogInfoMessage` faz parte do namespace `DynamoServices`, por isso, comece adicionando-o ao arquivo do projeto.

#### Etapa 2: Identificar quando registrar as informações <a href="#step-2-identify-when-to-log-information" id="step-2-identify-when-to-log-information"></a>

Antes de adicionar uma mensagem de informação, pense no propósito do método:

* Quais informações seriam úteis para confirmar após a conclusão de uma ação?
* Existem etapas ou marcos importantes no método que os usuários talvez queiram conhecer?

Exemplos de confirmações úteis:

* **Mensagens de conclusão** (por exemplo, quando uma grade ou um modelo é totalmente criado).
* **Detalhes dos dados processados** (por exemplo, “10 itens processados com êxito”).
* **Resumos de execução** (por exemplo, parâmetros usados no processo).

#### Etapa3: Usar `OnLogInfoMessage` para registrar mensagens informativas <a href="#step-3-use-onloginfomessage-to-log-informational-message" id="step-3-use-onloginfomessage-to-log-informational-message"></a>

Coloque chamadas `OnLogInfoMessage` em pontos significativos em seu método. Quando ocorrer uma etapa ou conclusão importante, registre uma mensagem informativa para atualizar o usuário sobre o que aconteceu.

### Sintaxe para `OnLogInfoMessage` <a href="#syntax-for-onloginfomessage" id="syntax-for-onloginfomessage"></a>

```
LogWarningMessageEvents.OnLogInfoMessage("Your info message here.");
```

### Exemplo de implementações de `OnLogInfoMessage` <a href="#example-implementations-of-onloginfomessage" id="example-implementations-of-onloginfomessage"></a>

Veja diferentes cenários para demonstrar usando `OnLogInfoMessage` nos nós Sem Toque.

#### Exemplo 1: Validar entradas numéricas <a href="#example-1-validating-numeric-inputs" id="example-1-validating-numeric-inputs"></a>

Neste exemplo, vamos nos basear no nó personalizado criado no **“Estudo de caso do nó Sem toque – Nó de grade”** anterior; um método denominado `RectangularGrid` que gera uma grade de retângulos com base em entradas `xCount` e `yCount`. Vamos percorrer o teste Se uma entrada é inválida e, em seguida, usar `OnLogInfoMessage` para fornecer informações após o nó ter concluído sua execução.

![Exemplo 1 de OnLogInfoMessage](images/onloginfomessage-example-1.png)

**Usar `OnLogInfoMessage` para validação de entrada**

Quando gerar uma grade com base em `xCount` e `yCount`. Depois de gerar a grade, recomenda-se confirmar sua criação registrando uma mensagem informativa com as dimensões da grade.

```
public static List<Rectangle> CreateGrid(int xCount, int yCount)
{
    var pList = new List<Rectangle>();
    // Grid creation code here...

    // Confirm successful grid creation
    LogWarningMessageEvents.OnLogInfoMessage($"Successfully created a grid with dimensions {xCount}x{yCount}.");

    return pList;
}
```

Neste exemplo:

* **Condição**: o processo de criação da grade foi concluído.
* **Mensagem**: `"Successfully created a grid with dimensions {xCount}x{yCount}."`

Essa mensagem informará aos usuários que a grade foi criada conforme especificado, ajudando-os a confirmar que o nó funcionou conforme o esperado.

Agora que sabemos como é, podemos implementá-lo no nó de exemplo Grades:

```
using Autodesk.DesignScript.Geometry;
using DynamoServices;

namespace CustomNodes
{
    public class Grids
    {
        // The empty private constructor.
        // This will not be imported into Dynamo.
        private Grids() { }

        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }

            // Log an info message indicating the grid was successfully created
            LogWarningMessageEvents.OnLogInfoMessage($"Successfully created a grid with dimensions {xCount}x{yCount}.");

            return pList;
        }
    }
}
```

#### Exemplo 2: Fornecer informações de contagem de dados <a href="#example-2-providing-data-count-information" id="example-2-providing-data-count-information"></a>

Se estiver criando um nó que processa uma lista de pontos, talvez seja necessário registrar quantos pontos foram processados com êxito. Isso pode ser útil para grandes conjuntos de dados.

![Exemplo 2 de OnLogInfoMessage](images/onloginfomessage-example-2.png)

```
public static List<Point> ProcessPoints(List<Point> points)
{
    var processedPoints = new List<Point>();
    foreach (var point in points)
    {
        // Process each point...
        processedPoints.Add(point);
    }

    // Log info about the count of processed points
    LogWarningMessageEvents.OnLogInfoMessage($"{processedPoints.Count} points were processed successfully.");

    return processedPoints;
}
```

Neste exemplo:

* **Condição**: após a conclusão do loop, mostrando a contagem de itens processados.
* **Mensagem**: `"6 points were processed successfully."`

Essa mensagem ajudará os usuários a entender o resultado do processamento e confirmar que todos os pontos foram processados.

#### Exemplo 3: Resumir os parâmetros usados <a href="#example-3-summarizing-parameters-used" id="example-3-summarizing-parameters-used"></a>

Em alguns casos, é útil confirmar os parâmetros de entrada de um nó usado para concluir uma ação. Por exemplo, se o nó exportar dados para um arquivo, o registro do nome e do caminho do arquivo poderá tranquilizar os usuários de que o arquivo correto foi usado.

![Exemplo 3 de OnLogInfoMessage](images/onloginfomessage-example-3.png)

```
public static void ExportData(string filePath, List<string> data)
{
    // Code to write data to the specified file path...

    // Log the file path used for export
    LogWarningMessageEvents.OnLogInfoMessage($"Data exported successfully to {filePath}.");

}
```

Neste exemplo:

* **Condição**: o processo de exportação é concluído com êxito.
* **Mensagem**: `"Data exported successfully to {filePath}."`

Essa mensagem confirma aos usuários que a exportação funcionou e mostra o caminho exato do arquivo, ajudando a evitar confusão sobre a localização dos arquivos.

## Criar e adicionar documentação personalizada a nós

### Documentação personalizada dos nós

Historicamente, houve limitações no Dynamo de como os autores de pacotes poderiam fornecer documentação para seus nós. Os autores dos nós personalizados foram restritos a permitir apenas uma breve descrição que é exibida na dica de ferramenta do nó ou a enviar seu pacote com gráficos de amostra muito anotados.

![Descrição da dica de ferramenta do nó](images/customnodedocumentation-overloads.png)

### Um novo modo

O Dynamo agora oferece um sistema melhorado para os autores de pacotes a fim de fornecer uma documentação melhor e mais explicativa para seus nós personalizados. Essa nova abordagem usa a linguagem Markdown fácil de usar para criação de texto e a extensão de vista do Navegador de documentação para exibir o Markdown no Dynamo. O uso de Markdown oferece aos autores de pacotes uma ampla gama de novas possibilidades ao documentar seus nós personalizados.

#### O que é o Markdown?

Markdown é uma linguagem de marcação leve que você pode usar para formatar documentos de texto sem formatação. Desde que o Markdown foi criado em 2004, sua popularidade só aumentou e agora é uma das linguagens de marcação mais usadas no mundo.

#### Guia de Introdução ao Markdown

É fácil começar a fazer arquivos Markdown – tudo o que você precisa é de um editor de texto simples, como o Bloco de notas, e você está pronto para começar. No entanto, há maneiras mais fáceis de escrever com Markdown do que usando o Bloco de notas. Existem vários editores on-line, como o [Dillinger](https://dillinger.io/), que permitem que você veja suas alterações em tempo real à medida que as faz. Outra forma muito usada de editar arquivos Markdown é usando um editor de código como o [Visual Studio Code](https://code.visualstudio.com/).

#### O que o Markdown pode fazer?

O Markdown é muito flexível e deve fornecer funcionalidade suficiente para criar facilmente uma boa documentação – isso inclui; adicionar arquivos de mídia como imagens ou vídeos, criar tabelas com diferentes formas de conteúdo e, claro, recursos de formatação de texto simples, como deixar o texto em **negrito** ou _itálico_. Tudo isso e muito mais é possível ao escrever documentos com Markdown. Para obter mais informações, veja este guia que explica a [sintaxe básica do Markdown](https://www.markdownguide.org/basic-syntax/).

### Adicionar documentação estendida aos nós

É fácil adicionar documentação aos nós. É possível adicionar documentação a todos os tipos de nós personalizados, incluindo:

* Nós de Dynamo prontos para uso
* Nós personalizados (.dyf) – Coleções de nós prontos para uso e/ou outros nós de pacote.
* Nós de pacote C# personalizados (também conhecidos como Zerotouch. Esses nós personalizados se parecem com os nós prontos para uso)
* Nós NodeModel (nós que contêm recursos especiais da interface do usuário, como menus suspensos ou botões de seleção)
* Nós NodeModel com interface do usuário personalizada (nós que contêm recursos exclusivos da interface do usuário, como gráficos no nó)

Siga estas etapas para exibir os arquivos Markdown no Dynamo.

#### Abrir arquivos de documentação no Dynamo

O Dynamo usa a extensão de vista do Navegador de documentação para exibir a documentação dos nós. Para abrir uma documentação de nós, clique com o botão direito do mouse no nó e selecione ajuda. Isso abrirá o Navegador de documentação e exibirá o Markdown associado a esse nó, se algum for fornecido.

![Navegação de documentação](images/customnodedocumentation-no-documentation-provided.png)

A documentação exibida no Navegador de documentação é composta de duas partes. A primeira é a seção `Node Info`, que é gerada automaticamente com base nas informações extraídas do nó, como as entradas/saídas, a categoria do nó, o nome/namespace do nó e a breve descrição dos nós. A segunda parte mostra a documentação de nós personalizados, que é o arquivo Markdown fornecido para documentar o nó.

![Documentação personalizada do nó](images/customnodedocumentation-custom-node-documentation.png)

#### Pasta doc do pacote

Para adicionar arquivos de documentação aos nós no Dynamo, crie uma nova pasta no diretório de pacotes chamado `/doc`. Quando o pacote for carregado, o Dynamo verificará esse diretório e obterá todos os arquivos Markdown de documentação nele.

#### Nomenclatura dos arquivos Markdown

Para garantir que o Dynamo saiba qual arquivo abrir quando um nó específico for solicitado, a nomenclatura do arquivo Markdown precisará estar em um formato específico. O arquivo Markdown deve ser nomeado de acordo com o nó que o namespace documenta. Se você não tiver certeza do namespace do nó, examine a seção `Node Info` quando pressionar `Help` no nó e, no nome do nó, você verá o namespace completo do nó selecionado.

Esse namespace deve ser o nome do arquivo Markdown desse nó específico. Por exemplo, o namespace de `CustomNodeExample` das imagens acima, é `TestPackage.TestCategory.CustomNodeExample`, portanto, o arquivo Markdown para esse nó deve ser nomeado `TestPackage.TestCategory.CustomNodeExample.md`

Em casos especiais em que você tem sobrecargas de nós (nós com o mesmo nome, mas entradas diferentes), será necessário adicionar os nomes de entrada em `()` após o namespace do nó. Por exemplo, o nó integrado `Geometry.Translate` apresenta várias sobrecargas. Nesse caso, nomearíamos os arquivos Markdown dos nós abaixo da seguinte maneira: `Autodesk.DesignScript.Geometry.Geometry.Translate(geometry,direction).md` `Autodesk.DesignScript.Geometry.Geometry.Translate(geometry,direction,distance).md`

![Nós de sobrecarga](images/customnodedocumentation-overloads.png)

#### Modificar arquivos Markdown enquanto abertos no Dynamo

Para facilitar a modificação de arquivos de documentação, o Navegador de documentação implementa um Inspetor de arquivos no arquivo de documentação aberto. Isso permite fazer alterações no arquivo Markdown e ver as alterações no Dynamo instantaneamente.

![Recarga dinâmica](images/customnodedocumentation-hot-reload.gif)

A adição de novos arquivos de documentação também pode ser feita enquanto o Dynamo está aberto. Basta adicionar um novo arquivo Markdown à pasta `/doc`, com um nome que corresponda ao nó que ele documenta.

## Adicionar ícones personalizados a nós Sem toque

### Visão geral

Os ícones personalizados para nós Sem toque no Dynamo tornam os nós visualmente distintos e mais fáceis de reconhecer na biblioteca. Ao adicionar ícones personalizados, é possível fazer com que seus nós se destaquem dos demais, permitindo que os usuários os identifiquem rapidamente em uma lista.

Este guia mostrará como adicionar ícones aos nós Sem Toque.

### Etapas para adicionar ícones de nós personalizados

#### Etapa 1: Configurar o projeto

Para começar, crie um projeto de biblioteca de classes do Visual Studio (.NET Framework) para os nós Sem Toque. Se ainda não tiver um projeto, consulte a seção **Introdução** para obter instruções passo a passo sobre como criar um.

![Criar um projeto do Visual Studio](images/vs-new-project-1.jpg)

![Configurar um novo projeto no Visual Studio](images/zerotouchicons-configure-new-project.jpg)

Certifique-se de ter pelo menos um nó Sem toque funcional, pois os ícones só podem ser adicionados aos nós existentes. Para obter orientação, consulte o **Estudo de caso do nó Sem toque - Nó de grade**.

#### Etapa 2: Criar as imagens dos ícones

Para criar ícones personalizados:

1. **Criar os ícones**: use um editor de imagem para criar ícones simples e visualmente claros para os nós.
2. **Especificações das imagens**:
   * **Ícone pequeno**: 32 x 32 pixels (usado na barra lateral da Biblioteca e no próprio nó).
   * **Ícone grande**: 128 x 128 pixels (usado nas propriedades do nó ao passar o cursor sobre o nó na biblioteca).
3. **Convenção de nomenclatura**:
   * Os nomes de arquivo devem coincidir com o formato abaixo para associá-los ao nó correto:
     * **`<ProjectName>.<ClassName>.<MethodName>.Small.png`** (para o ícone pequeno).
     * **`<ProjectName>.<ClassName>.<MethodName>.Large.png`** (para o ícone grande).

**Exemplo**: se o projeto for `ZeroTouchNodeIcons`, a classe for `Grids` e o método for `RectangularGrid`, os arquivos deverão ser nomeados:

* `ZeroTouchNodeIcons.Grids.RectangularGrid.Small.png`
* `ZeroTouchNodeIcons.Grids.RectangularGrid.Large.png`

> Dica: Mantenha um tema de design consistente em todos os ícones para obter uma aparência profissional.

#### Etapa 3: Adicionar um arquivo de recursos ao projeto

Para incorporar os ícones no `.dll`, crie um arquivo de recursos:

1. **Adicionar um novo arquivo de recursos**:

* Clique com o botão direito do mouse no projeto no **Gerenciador de soluções**.

![Adicionar um novo item](images/zerotouchicons-add-resources-file-1.jpg)

* Acesse **Adicionar > Novo item** e selecione **Arquivo de recursos**.

![Adicionar um arquivo de recursos](images/zerotouchicons-add-resources-file-2.jpg)

* Nomeie o arquivo `<ProjectName>Images.resx`. Por exemplo, `ZeroTouchNodeIconsImages.resx`.

2. **Desmarque a propriedade Ferramenta personalizada**:
   * Selecione o arquivo de recursos no **Gerenciador de soluções**.
   * No painel **Propriedades**, limpe o campo `Custom Tool` removendo o valor `ResXFileCodeGenerator`.

![Limpar a propriedade Ferramenta personalizada](images/zerotouchicons-custom-tool-property.jpg)

> _Observação: Não limpar o campo “Ferramenta personalizada” fará com que o Visual Studio converta os pontos em sublinhados nos nomes de recurso. Verifique antes de construir se os nomes dos recursos têm pontos separando os nomes de classe em vez de sublinhados._

#### Etapa 4: Adicionar as imagens como recursos

1. Abra o arquivo de recursos usando o **Editor de recursos gerenciados (herdado)**:
   * Se estiver usando o Visual Studio 17.11 ou posterior, clique com o botão direito do mouse no arquivo de recursos, escolha **Abrir com** e selecione **Editor de recursos gerenciados (herdado)**.
   * Se estiver usando uma versão do Visual Studio anterior à 17.11, clique duas vezes no arquivo de recursos para abrir com o Editor de recursos (que em sua versão do Visual Studio ainda não foi tornada herdada).

![Usar Abrir com...](images/zerotouchicons-open-resource-editor.jpg)

![Abrir o arquivo de recursos com o Editor de recursos gerenciados (herdado)](images/zerotouchicons-managed-resource-editor-legacy.jpg)

2. Adicione as imagens:
   * Arraste e solte os arquivos de imagem no editor ou use a opção **Adicionar arquivo existente**.

![Adicionar arquivos existentes](images/zerotouchicons-add-existing-file.jpg)

3. Atualize a persistência:
   * Selecione as imagens no Editor de recursos (isso não funcionará se forem selecionadas no Gerenciador de soluções), altere a propriedade **Persistência** no painel **Propriedades** para `Embedded in .resx`. Isso garante que as imagens sejam incluídas no `.dll`.

![Atualizar a persistência](images/zerotouchicons-edit-persistence-property.jpg)

#### Etapa 5: Converter o projeto no estilo SDK

Se o projeto ainda não estiver no estilo SDK (necessário para incorporar recursos), converta-o:

1. Instale a extensão `.NET Upgrade Assistant` no menu **Extensões > Gerenciar extensões** do Visual Studio.

![Gerenciar extensões](images/zerotouchicons-manage-extensions.jpg)

![Instalar o Assistente de atualização do .NET](images/zerotouchicons-net-upgrade-assistant.jpg)

2. Clique com o botão direito do mouse no projeto no **Gerenciador de soluções** e selecione **Atualizar > Converter projeto em estilo SDK**.

![Atualizar o projeto](images/zerotouchicons-upgrade-project.jpg)

![Converter em estilo SDK](images/zerotouchicons-convert-to-sdk-style.jpg)

3. Aguarde a conclusão da conversão.

![Atualização concluída](images/zerotouchicons-upgrade-complete.jpg)

#### Etapa 6: Adicionar um script After-Build para incorporar recursos

1. Descarregue o projeto:
   * Clique com o botão direito do mouse no projeto no **Gerenciador de soluções** e selecione **Descarregar projeto**.

![Descarregar o projeto](images/zerotouchicons-unload-project.jpg)

2. Edite o arquivo `.csproj`:
   * Adicione o seguinte elemento `<Target>` entre `</ItemGroup>` e `</Project>`:

```
<Target Name="CreateNodeIcons" AfterTargets="PostBuildEvent">
		<!-- Get System.Drawing.dll     -->
		<GetReferenceAssemblyPaths TargetFrameworkMoniker=".NETFramework, Version=v4.8">
			<Output TaskParameter="FullFrameworkReferenceAssemblyPaths" PropertyName="FrameworkAssembliesPath" />
		</GetReferenceAssemblyPaths>
		<!-- Get assembly -->
		<GetAssemblyIdentity AssemblyFiles="$(OutDir)$(TargetName).dll">
			<Output TaskParameter="Assemblies" ItemName="AssemblyInfo" />
		</GetAssemblyIdentity>
		<!-- Generate customization dll -->
		<GenerateResource SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)" UseSourcePath="true" Sources="$(ProjectDir)ZeroTouchNodeIconsImages.resx" OutputResources="$(ProjectDir)ZeroTouchNodeIconsImages.resources" References="$(FrameworkAssembliesPath)System.Drawing.dll" />
		<AL SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)" TargetType="library" EmbedResources="$(ProjectDir)ZeroTouchNodeIconsImages.resources" OutputAssembly="$(OutDir)ZeroTouchNodeIcons.customization.dll" Version="%(AssemblyInfo.Version)" />
	</Target>
```

![Adicionar o código After-Build](images/zerotouchicons-after-build.jpg)

1. Substitua todas as instâncias de `ZeroTouchNodeIcons` pelo nome do projeto.
2. Recarregue o projeto:
   * Clique com o botão direito do mouse no projeto descarregado e selecione **Recarregar projeto**.

![Recarregar o projeto](images/zerotouchicons-reload-project.jpg)

#### Etapa 7: Construir e carregar o .dll no Dynamo

1. Construir o projeto:
   * Depois de adicionar o script After-Build, construa o projeto no Visual Studio.

![Construir a solução](images/zerotouchicons-build-solution.jpg)

2. Verifique os arquivos de saída:
   * Verifique se o `.dll` e o `.customization.dll` estão na pasta `bin`.
3. Adicione o `.dll` ao Dynamo:
   * No Dynamo, use o botão Importar biblioteca para importar o .dll para o Dynamo.

![Botão Importar biblioteca](images/zerotouchicons-icon-in-dynamo.jpg)

4. Os nós personalizados agora devem aparecer com seus respectivos ícones.
