# Interface do usuário

### Visão geral da interface do usuário

A interface do usuário (IU) do Dynamo é organizada em cinco regiões principais. Abordaremos brevemente a visão geral aqui e explicaremos melhor sobre o espaço de trabalho e a biblioteca nas seções a seguir.

\![](<../.gitbook/assets/user interface - ui.jpg>)

> 1. Menus
> 2. Barra de ferramentas
> 3. Biblioteca
> 4. Espaço de trabalho
> 5. Barra de execução

### Menus

![](../.gitbook/assets/userinterface-menu\(1\).jpg)

Confira aqui os menus para a funcionalidade básica do aplicativo Dynamo. Como a maioria dos softwares do Windows, os dois primeiros menus estão relacionados ao gerenciamento de arquivos, às operações de seleção e à edição de conteúdos. Os menus restantes são mais específicos do Dynamo.

#### Menus do Dynamo

As informações gerais e as configurações podem ser encontradas no menu suspenso do **Dynamo**.

\![](<../.gitbook/assets/user interface - dynamo menu.jpg>)

> 1. Sobre – Descubra a versão do Dynamo instalada no computador.
> 2. Contrato para coletar dados de usabilidade – Isso permite que você aceite, ou não, o compartilhamento dos dados de usuário para aprimorar o Dynamo.
> 3. Preferências – Inclui configurações como a definição da precisão decimal do aplicativo e a qualidade de renderização da geometria.
> 4. Sair do Dynamo

#### Ajuda

Se você tiver dúvidas, confira o menu **Ajuda**. Você pode acessar um dos sites de referência do Dynamo através do navegador da Internet.

![](../.gitbook/assets/help-menu.png)

> 1. Guias interativos – Tours que guiam você passo a passo através de vários recursos do Dynamo.
> 2. Amostras – Arquivos de exemplo de referência. Disponíveis somente nos programas hospedeiros, incluindo o Revit e o Civil 3D.
> 3. Dicionário do Dynamo – Recurso com a documentação sobre todos os nós.
> 4. Site do Dynamo – Um site de informações sobre o Dynamo e links para recursos como o fórum, o blog etc.
> 5. Repositório do Dynamo – Visualize o projeto do Dynamo no GitHub. 
> 6. Wiki do projeto do Dynamo – Consulte a wiki para saber como desenvolver usando a API do Dynamo, com suporte a bibliotecas e ferramentas.
> 7. Exibir a página inicial – Retorna para a página inicial do Dynamo quando dentro de um documento.
> 8. Relatório de um bug – Abre um problema no GitHub.

### Barra de ferramentas

A barra de ferramentas do Dynamo contém uma série de botões para acesso rápido ao trabalho com arquivos, bem como os comandos Desfazer [Ctrl + Z] e Refazer [Ctrl + Y]. Na parte mais à direita, há outro botão que exporta um instantâneo do espaço de trabalho, o que é extremamente útil para fins de documentação e compartilhamento.

* \![](<../.gitbook/assets/user interface - new file.jpg>) Novo – Criar um novo arquivo .dyn
* \![](<../.gitbook/assets/user interface - open (1).jpg>) Abrir – Abrir um arquivo .dyn (espaço de trabalho) ou .dyf (nó personalizado) existente
* \![](<../.gitbook/assets/user interface - save.jpg>) Salvar/Salvar como – Salvar o arquivo .dyn ou .dyf ativo
* \![](<../.gitbook/assets/user interface - undo.jpg>) Desfazer – Desfazer a última ação
* \![](<../.gitbook/assets/user interface - redo.jpg>) Refazer – Refazer a última ação
* \![](<../.gitbook/assets/user interface - screenshot.jpg>) Exportar espaço de trabalho como imagem – Exportar o espaço de trabalho visível como um arquivo PNG

### Biblioteca

A biblioteca do Dynamo é um conjunto de bibliotecas funcionais e cada biblioteca contém nós agrupados por categoria. Trata-se de bibliotecas básicas que são adicionadas durante a instalação padrão do Dynamo. À medida que desenvolvemos seu uso, demonstraremos como estender a funcionalidade básica com nós personalizados e pacotes adicionais. A seção [2-library.md](2-library.md "menção") oferece uma orientação mais detalhada sobre esse uso.

\![](<../.gitbook/assets/user interface - library (1).gif>)

### Espaço de trabalho

O espaço de trabalho é onde desenvolvemos nossos programas visuais. Também é possível alterar aqui a configuração de visualização para visualizar as geometrias 3D. Consulte [1-workspace.md](1-workspace.md "menção") para obter mais detalhes.

\![](<../.gitbook/assets/user interface - workspace (1).gif>)

### Barra de execução

Execute o script do Dynamo daqui. Clique no ícone do menu suspenso no botão Executar para alternar entre os diferentes modos.

\![](<../.gitbook/assets/user interface - execution bar.gif>)

* Automático: executa o script automaticamente. As alterações são atualizadas em tempo real.
* Manual: o script somente é executado ao clicar no botão “Executar”. Isso é útil para fazer alterações em scripts “pesados” e complicados.
* Periódico: essa opção está desativada por padrão. Somente está disponível quando o nó _DateTime.Now_ é usado. É possível definir o gráfico para ser executado automaticamente em um intervalo especificado.

\![](<../.gitbook/assets/user interface - execution bar DateTime node.jpg>)
