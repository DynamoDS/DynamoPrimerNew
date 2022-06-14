# Interface do usuário

### Visão geral da interface do usuário

A interface do usuário (IU) do Dynamo é organizada em cinco regiões principais. Abordaremos brevemente a visão geral aqui e explicaremos melhor sobre o espaço de trabalho e a biblioteca nas seções a seguir.

![](<images/user interface - ui.jpg>)

> 1. Menus
> 2. Barra de ferramentas
> 3. Biblioteca
> 4. Área de trabalho
> 5. Barra de execução

### Menus

![](<images/user interface - menu.jpg>)

Confira aqui os menus para a funcionalidade básica do aplicativo Dynamo. Como a maioria dos softwares do Windows, os dois primeiros menus estão relacionados ao gerenciamento de arquivos, às operações de seleção e à edição de conteúdos. Os menus restantes são mais específicos do Dynamo.

#### Menus do Dynamo

Informações gerais e configurações podem ser encontradas no menu suspenso do **Dynamo**.

![](<images/user interface - dynamo menu.jpg>)

> 1. Sobre – Descubra a versão do Dynamo instalada no computador.
> 2. Contrato para coletar dados de usabilidade – Isso permite que você aceite, ou não, o compartilhamento dos dados de usuário para aprimorar o Dynamo.
> 3. Preferências – Inclui configurações como a definição da precisão decimal do aplicativo e a qualidade de renderização da geometria.
> 4. Sair do Dynamo

#### Ajuda

Se você tiver dúvidas, confira o menu **Ajuda**. Você pode acessar um dos sites de referência do Dynamo através do navegador da Internet.

![](<images/user interface - help menu.jpg>)

> 1. Introdução: uma breve introdução sobre a utilização do Dynamo.
> 2. Guias interativos –
> 3. Amostras: arquivos de exemplo de referência.
> 4. Dicionário do Dynamo – Recurso com a documentação sobre todos os nós.
> 5. Site do Dynamo – Visualizar o projeto do Dynamo no GitHub.
> 6. Wiki do projeto do Dynamo – Visite a wiki para saber como desenvolver usando a API do Dynamo, com suporte a bibliotecas e ferramentas.
> 7. Exibir a página inicial: retorna para a página inicial do Dynamo quando dentro de um documento.
> 8. Relatório de um bug: abre um problema no GitHub.

### Barra de ferramentas

A barra de ferramentas do Dynamo contém uma série de botões para acesso rápido ao trabalho com arquivos, bem como os comandos Desfazer [Ctrl + Z] e Refazer [Ctrl + Y]. Na parte mais à direita há outro botão que exportará um instantâneo do espaço de trabalho, o que é extremamente útil para a documentação e o compartilhamento.

* ![](<images/user interface - new file.jpg>) Novo – Cria um novo arquivo .dyn
* ![](<images/user interface - open (1).jpg>) Abrir – Abre um arquivo .dyn (espaço de trabalho) ou .dyf (nó personalizado) existente
* ![](<images/user interface - save.jpg>) Salvar/Salvar como – Salva o arquivo .dyn ou .dyf ativo
* ![](<images/user interface - undo.jpg>) Desfazer – Desfaz a última ação
* ![](<images/user interface - redo.jpg>) Refazer – Refaz a ação seguinte
* ![](<images/user interface - screenshot.jpg>) Exportar espaço de trabalho como imagem – Exporta o espaço de trabalho visível como um arquivo PNG

### Biblioteca

A biblioteca do Dynamo é um conjunto de bibliotecas funcionais e cada biblioteca contém nós agrupados por categoria. Trata-se de bibliotecas básicas que são adicionadas durante a instalação padrão do Dynamo. À medida que desenvolvemos seu uso, demonstraremos como estender a funcionalidade básica com nós personalizados e pacotes adicionais. A seção [2-library.md](2-library.md "menção") oferece uma orientação mais detalhada sobre esse uso.

![](<images/user interface - library.jpg>)

### Área de trabalho

O espaço de trabalho é onde desenvolvemos nossos programas visuais. Também é possível alterar aqui a configuração de visualização para visualizar as geometrias 3D. Consulte a [1-workspace.md](1-workspace.md "menção") para obter mais detalhes.

![](<images/user interface - workspace.gif>)

### Barra de execução

Execute o script do Dynamo daqui. Clique no ícone do menu suspenso no botão Executar para alterar entre os diferentes modos.

![](<images/user interface - execution bar.gif>)

* Automático: executa o script automaticamente. As alterações são atualizadas em tempo real.
* Manual: o script somente é executado ao clicar no botão “Executar”. Isso é útil para fazer alterações em “scripts pesados” e complicados
* Periódico: essa opção está desativada por padrão. Somente está disponível quando o nó DateTime.Now é usado. É possível definir o gráfico para ser executado automaticamente em um intervalo especificado.

![](<images/user interface - execution bar DateTime node.jpg>)
