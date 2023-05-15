# Desenvolvimento do Dynamo

Independentemente do nível de experiência, a plataforma do Dynamo foi projetada para que todos os usuários sejam colaboradores. Há diversas opções de desenvolvimento voltadas para diferentes habilidades e níveis de habilidades, cada uma com seus pontos fortes e fracos, dependendo do objetivo. Abaixo, descreveremos as diferentes opções e como escolher uma em detrimento de outra.

![Três ambientes de desenvolvimento](images/developing-for-dynamo.png)

> Três ambientes de desenvolvimento: Visual Studio, Editor do Python e DesignScript com Blocos de código

#### Quais são as minhas opções? <a href="#what-are-my-options" id="what-are-my-options"></a>

As opções de desenvolvimento do Dynamo são divididas principalmente em duas categorias: _para_ o Dynamo versus _no_ Dynamo. As duas categorias podem ser consideradas semelhantes; “no” Dynamo implica o conteúdo criado usando o IDE do Dynamo para ser usado no Dynamo e “para” o Dynamo implica o uso de ferramentas externas para criar conteúdo a ser importado para o Dynamo para ser usado. Embora este guia se concentre no desenvolvimento _para_ o Dynamo, os recursos para todos os processos são descritos abaixo.

#### Para o Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Estes nós permitem o maior grau de personalização. Muitos pacotes são criados com esse método e é necessário para contribuir para a origem do Dynamo. O processo de compilação deles será abordado neste guia.

* Nós Sem toque
* Nós derivados do NodeModel
* Extensões

> O Manual tem um guia sobre como [Importar bibliotecas Sem toque](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

Para a discussão abaixo, o Visual Studio é usado como o ambiente de desenvolvimento para os nós Sem toque e NodeModel.

![Interface do Visual Studio](images/vs-devenv.jpg)

> A interface do Visual Studio com um projeto que desenvolveremos

#### No Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Embora esses processos existam no espaço de trabalho de programação visual e sejam relativamente diretos, todos eles são opções viáveis para personalizar o Dynamo. O Manual cobre esses tópicos extensivamente e fornece dicas de scripts e as melhores práticas no capítulo [Estratégias de script](http://dynamoprimer.com/en/12\_Best-Practice/12-1\_Scripting-Strategies.html).

*   Os Blocos de código expõem o DesignScript no ambiente de programação visual, permitindo fluxos de trabalho flexíveis de scripts de texto e nós. Uma função em um bloco de código pode ser chamada por qualquer item no espaço de trabalho.

    > Faça o download de um exemplo de bloco de código (clique com o botão direito do mouse e salve como) ou veja um percurso virtual detalhado no [Manual](https://primer.dynamobim.org/07\_Code-Block/7-1\_what-is-a-code-block.html).
*   Os nós personalizados são contêineres para coleções de nós ou até mesmo gráficos inteiros. Eles são uma forma eficaz de coletar rotinas usadas com frequência e compartilhá-las com a comunidade.

    > Faça o download de um exemplo de nó personalizado (clique com o botão direito do mouse e salve como) ou veja um percurso virtual detalhado no [Manual](https://primer.dynamobim.org/10\_Custom-Nodes/10-1\_Introduction.html).
*   Os nós Python são uma interface de scripts no espaço de trabalho de programação visual, semelhante aos blocos de código. As bibliotecas Autodesk.DesignScript usam uma notação de ponto similar ao DesignScript.

    > Faça o download de um exemplo de nó Python (clique com o botão direito do mouse e salve como) ou veja um percurso virtual detalhado no [Manual](https://primer.dynamobim.org/10\_Custom-Nodes/10-4\_Python.html)

O desenvolvimento no espaço de trabalho do Dynamo é uma ferramenta poderosa para obter feedback imediato.

![Desenvolvimento no espaço de trabalho do Dynamo com o nó Python](images/python-example.jpg)

> Desenvolvimento no espaço de trabalho do Dynamo com o nó Python

#### Quais são as vantagens/desvantagens de cada um? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

As opções de desenvolvimento do Dynamo foram projetadas para lidar com a complexidade de uma necessidade de personalização. Se o objetivo é escrever um script recursivo no Python ou compilar uma interface de usuário de nó totalmente personalizada, há opções para implementar código que envolvem apenas o que é necessário para começar a trabalhar.

**Blocos de código, nó Python e nós personalizados no Dynamo**

Essas são opções simples para escrever código no ambiente de programação visual do Dynamo. O espaço de trabalho de programação visual do Dynamo fornece acesso ao Python, ao DesignScript e à capacidade de conter vários nós dentro de um nó personalizado.

![Bloco de código, script Python e nó personalizado](images/Development-Icons.png)

Com esses métodos, podemos:

* Começar a escrever Python ou DesignScript com pouca ou nenhuma configuração.
* Importar bibliotecas Python para o Dynamo.
* Compartilhar blocos de código, nós Python e nós personalizados com a comunidade do Dynamo como parte de um pacote.

**Nós Sem toque**

O nó Sem toque é um método simples de apontar e clicar para importar bibliotecas C#. O Dynamo lê os métodos públicos de um `.dll` e os converte em nós do Dynamo. É possível usar o nó Sem toque para desenvolver seus próprios nós e pacotes personalizados.

![Nós Sem toque](images/ZTImport.png)

Com esse método, podemos:

* Importar uma biblioteca que não foi necessariamente desenvolvida para o Dynamo e criar automaticamente um conjunto de novos nós, como o [exemplo A-Forge](http://dynamoprimer.com/en/10\_Packages/10-5\_Zero-Touch.html) no Manual
* Escrever métodos C# e usar facilmente os métodos como nós no Dynamo
* Compartilhar uma biblioteca C# como nós com a comunidade do Dynamo em um pacote

**Nós derivados do NodeModel**

Esses nós são um passo mais profundo na estrutura do Dynamo. Eles são baseados na classe `NodeModel` e escritos em C#. Embora esse método forneça a maior flexibilidade e potência, a maioria dos aspectos do nó precisa ser explicitamente definida e as funções precisam residir em uma montagem separada.

![Nós derivados do NodeModel](images/Development-Icons-NodeModel.png)

Com esse método, podemos:

* Criar uma interface de usuário de nó totalmente personalizável com controles deslizantes, imagens, cores etc. (por exemplo, nó ColorRange)
* Acessar e afetar o que está acontecendo na tela do Dynamo
* Personalizar a amarra
* Carregar no Dynamo como um pacote

#### Entender o controle de versão do Dynamo e as alterações da API (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Como o Dynamo está sendo atualizado regularmente, as alterações podem ser feitas em parte da API que um pacote usa. O rastreamento dessas alterações é importante para garantir que os pacotes existentes continuem a funcionar corretamente.

As alterações da API são rastreadas no [Wiki do Dynamo Github](https://github.com/DynamoDS/Dynamo/wiki/API-Changes). Isso abrange as alterações no DynamoCore, nas bibliotecas e nos espaços de trabalho.

![A API do Dynamo altera o documento](images/api-changes.jpg)

Um exemplo de uma mudança significativa futura é a transição do formato de arquivo XML para o formato JSON na versão 2.0. Os nós derivados do NodeModel agora precisarão de um [construtor JSON](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node); caso contrário, eles não serão abertos no Dynamo 2.0.

A documentação da API do Dynamo atualmente cobre a funcionalidade principal: [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI)

![Documentação da API](images/api-docs.jpg)

#### Permissão para distribuir binários em um pacote <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Esteja ciente de que os .dll estão incluídos em um pacote que está sendo carregado no gerenciador de pacotes. Se o autor do pacote não tiver criado o .dll, ele deverá ter os direitos para compartilhá-lo.

Se um pacote incluir binários, os usuários deverão ser solicitados ao fazer o download de que o pacote contém binários.
