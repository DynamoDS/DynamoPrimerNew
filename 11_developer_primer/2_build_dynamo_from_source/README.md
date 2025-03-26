# Compilar o Dynamo da origem

A origem do Dynamo está hospedada no Github para que qualquer pessoa possa clonar e fazer contribuições. Neste capítulo, analisaremos como clonar o repositório usando o git, compilar os arquivos de origem com o Visual Studio, executar e depurar uma compilação local e extrair as novas alterações do Github.

### Localizar os repositórios do Dynamo no Github <a href="#locating-the-dynamo-repositories-on-github" id="locating-the-dynamo-repositories-on-github"></a>

O Github é um serviço de hospedagem baseado no [git](https://docs.github.com/pt/get-started/quickstart/git-and-github-learning-resources), um sistema de controle de versão para rastrear alterações e coordenar o trabalho entre as pessoas. O Git é uma ferramenta que podemos aproveitar para fazer download dos arquivos de origem do Dynamo e mantê-los atualizados com alguns comandos. O uso desse método evitará o trabalho desnecessário e inerentemente confuso de fazer o download e substituir manualmente os arquivos de origem em cada atualização. O sistema de controle de versão git rastreará as diferenças entre um repositório de código local e remoto.

A origem do Dynamo está hospedada no DynamoDS Github neste repositório: [https://github.com/DynamoDS/Dynamo](https://github.com/DynamoDS/Dynamo)

![Arquivos de origem do Dynamo](images/github.jpg)
> Arquivos de origem do Dynamo.
>
> 1. Clonar ou fazer o download de todo o repositório
> 2. Visualizar outros repositórios do DynamoDS
> 3. Arquivos de origem do Dynamo
> 4. Arquivos específicos do Git

### Extrair o repositório do Dynamo usando o git <a href="#pulling-the-dynamo-repository-using-git" id="pulling-the-dynamo-repository-using-git"></a>

Antes de clonar o repositório, precisamos instalar o git. Siga este [breve guia](https://docs.github.com/pt/get-started/quickstart/set-up-git#setting-up-git) para ver as etapas de instalação e como configurar um nome de usuário e e-mail no GitHub. Neste exemplo, usaremos o git na linha de comando. Este guia pressupõe que você esteja usando o Windows, mas também pode usar o git no Mac ou Linux para clonar a origem do Dynamo.

Precisamos de uma URL para o repositório do Dynamo para clonar. Essa URL pode ser encontrada no botão “Clonar ou fazer o download” na página do repositório. Copie a URL para colar no prompt do comando.

![Clonar um repositório](images/github-clone.png)
> 1. Selecionar “Clonar ou fazer o download”
> 2. Copiar a URL

Com o git instalado, podemos clonar o repositório do Dynamo. Comece abrindo o prompt do comando. Em seguida, use o comando de alteração de diretório `cd` para navegar até a pasta na qual desejamos que os arquivos de origem sejam clonados. Neste caso, criamos uma pasta chamada `Github` em `Documents`.

`cd C:\Users\username\Documents\GitHub`

> Substitua “username” por seu nome de usuário

![Prompt do comando](images/cli-1.jpg)

Na próxima etapa, executaremos um comando git para clonar o repositório do Dynamo para o local que especificamos. A URL no comando é obtida clicando no botão “Clonar ou fazer o download” no Github. Execute esse comando no terminal de comando. Observe que isso clonará a ramificação mestre de repositório do Dynamo, que é o código mais atualizado do Dynamo, e conterá a versão mais recente do código do Dynamo. Essa ramificação muda diariamente.

`git clone https://github.com/DynamoDS/Dynamo.git`

![Resultados da operação de clonagem do Git](images/cli-2.jpg)

Saberemos que o git está funcionando se a operação de clonagem tiver sido concluída com êxito. No gerenciador de arquivos, navegue até o diretório onde você clonou para ver os arquivos de origem. A estrutura de diretórios deve ser idêntica à ramificação mestre do repositório do Dynamo no Github.

![Arquivos de origem do Dynamo](images/source-files.jpg)

> 1. Arquivos de origem do Dynamo
> 2. Arquivos do Git

### Compilar o repositório usando o Visual Studio <a href="#building-the-repository-using-visual-studio" id="building-the-repository-using-visual-studio"></a>

Com os arquivos de origem agora clonados em nosso computador local, podemos compilar um arquivo executável para o Dynamo. Para fazer isso, precisamos configurar o Visual Studio IDE e assegurar que o .NET Framework e o DirectX estejam instalados.

* Faça o download e instale o [Microsoft Visual Studio Community 2015](https://my.visualstudio.com/Downloads/Results), um IDE gratuito e completo (ambiente de desenvolvimento integrado – as versões posteriores também podem funcionar)
* Faça o download e instale o [Microsoft .NET Framework 4.5](https://www.microsoft.com/pt-br/download/details.aspx?id=30653) ou posterior
* Instale o Microsoft DirectX do repositório local do Dynamo (`Dynamo\tools\install\Extra\DirectX\DXSETUP.exe`)

> O .NET e o DirectX podem já estar instalados.

Quando tudo estiver pronto, podemos iniciar o Visual Studio e abrir a solução `Dynamo.All.sln` localizada em `Dynamo\src`.

![Abrir o arquivo da solução](images/vs-open-dynamo.jpg)

> 1. Selecionar `File > Open > Project/Solution`
> 2. Navegar até o repositório do Dynamo e abrir a pasta `src`
> 3. Selecionar o arquivo da solução `Dynamo.All.sln`
> 4. Selecionar `Open`

Antes de podermos compilar a solução, algumas configurações devem ser especificadas. Primeiro, devemos compilar uma versão de depuração do Dynamo para que o Visual Studio possa coletar mais informações durante a depuração para nos ajudar a desenvolver, e queremos a AnyCPU como destino.

![Configurações da solução](images/vs-dynamo-build-settings.jpg)

> Elas se tornarão pastas dentro da pasta `bin`
>
> 1. Para este exemplo, escolhemos `Debug` como a Configuração da solução
> 2. Definir a Plataforma de soluções como `Any CPU`

Com o projeto aberto, podemos compilar a solução. Esse processo criará um arquivo DynamoSandbox.exe que podemos executar.

![Compilar a solução](images/vs-build-dynamo.jpg)

> A compilação do projeto restaurará as dependências do NuGet.
>
> 1. Selecionar `Build > Build Solution`
> 2. Verificar se a compilação foi bem-sucedida na janela Saída. Deve ser semelhante a `==== Build: 69 succeeded, 0 failed, 0 up-to-date, 0 skipped ====`

### Executar uma compilação local <a href="#running-a-local-build" id="running-a-local-build"></a>

Se o Dynamo for compilado com êxito, uma pasta `bin` será criada no repositório do Dynamo com o arquivo DynamoSandbox.exe. No nosso caso, estamos compilando com a opção Depurar, portanto, o arquivo executável estará localizado em `bin\AnyCPU\Debug`. A execução dessa ação abrirá uma compilação local do Dynamo.

![Executável do DynamoSandbox](images/ex-dynamosandbox.jpg)

> 1. O executável do DynamoSandbox que acabamos de criar. Execute isso para iniciar o Dynamo.

Agora estamos quase totalmente preparados para começar a desenvolver o Dynamo.

Para obter instruções sobre como compilar o Dynamo para outras plataformas (por exemplo, Linux ou OS X), visite esta [página wiki](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-on-Linux,-Mac).

### Depurar uma compilação local usando o Visual Studio <a href="#debugging-a-local-build-using-visual-studio" id="debugging-a-local-build-using-visual-studio"></a>

A depuração é um processo de identificação, isolamento e correção de um problema ou bug. Após o Dynamo ter sido criado com êxito da origem, poderemos usar diversas ferramentas no Visual Studio para depurar um aplicativo em execução, por exemplo, o complemento DynamoRevit. Podemos analisar seu código-fonte para localizar a causa de um problema ou observar o código em execução no momento. Para obter uma explicação mais detalhada de como depurar e navegar no código no Visual Studio, consulte o [Documentos do Visual Studio](https://docs.microsoft.com/pt-br/visualstudio/debugger/navigating-through-code-with-the-debugger).

Para o aplicativo independente do Dynamo, DynamoSandbox, abordaremos duas opções para depuração:

* Compilar e iniciar o Dynamo diretamente no Visual Studio
* Anexar o Visual Studio a um processo em execução do Dynamo

Iniciar o Dynamo no Visual Studio recompila a solução para cada sessão de depuração, se necessário; portanto, se fizermos alterações na origem, elas serão incorporadas ao depurar. Com a solução `Dynamo.All.sln` ainda aberta, selecione `Debug`, `AnyCPU` e `DynamoSandbox` nos menus suspensos e clique em `Start`. Isso compilará o Dynamo, iniciará um novo processo (DynamoSandbox.exe) e anexará o depurador do Visual Studio a ele.

![Compilar e iniciar o aplicativo no Visual Studio](images/vs-debug-options.jpg)

> Compilar e iniciar o aplicativo diretamente no Visual Studio
>
> 1. Definir a configuração como `Debug`
> 2. Definir a plataforma como `Any CPU`
> 3. Definir o projeto de inicialização como `DynamoSandbox`
> 4. Clicar em `Start` para iniciar o processo de depuração

Como alternativa, podemos desejar depurar um processo do Dynamo que já está em execução para solucionar um problema com um gráfico ou pacote específico aberto. Para essa finalidade, abriríamos os arquivos de origem do projeto no Visual Studio e anexaríamos a um processo do Dynamo em execução usando o item de menu de depuração `Attach to Process`.

![Caixa de diálogo Anexar ao processo](images/vs-attach-dynamosandbox.jpg)

> Anexar um processo em execução ao Visual Studio
>
> 1. Selecionar `Debug > Attach to Process...`
> 2. Escolher `DynamoSandbox.exe`
> 3. Selecionar `Attach`

Em ambas as situações, estamos anexando o depurador a um processo que desejamos depurar. Podemos definir pontos de quebra no código antes ou após iniciar o depurador que fará com que o processo pause imediatamente antes de executar essa linha de código. Se uma exceção não capturada for gerada durante a depuração, o Visual Studio pulará para onde ela ocorreu no código-fonte. Esse é um método eficiente para localizar falhas simples e exceções não tratadas e também para entender o fluxo de execução de um aplicativo.

![Definir um ponto de quebra](images/vs-debug-dynamocore.jpg)

> Durante a depuração do DynamoSandbox, definimos um ponto de quebra no construtor do nó Color.ByARGB que faz com que o processo do Dynamo pause quando o nó é instanciado. Se esse nó estava gerando uma exceção ou causando um erro fatal no Dynamo, poderíamos passar por cada linha no construtor para descobrir onde o problema estava ocorrendo.
>
> 1. O ponto de quebra
> 2. A pilha de chamadas mostrando a função que está sendo executada no momento e as chamadas de função anteriores.

Na próxima seção, **Compilar o DynamoRevit da origem**, analisaremos um exemplo específico de depuração e explicaremos como definir pontos de quebra, passar pelo código e ler a pilha de chamadas.

### Extrair a compilação mais recente <a href="#pulling-latest-build" id="pulling-latest-build"></a>

Como a origem do Dynamo está hospedada no Github, a maneira mais fácil de manter os arquivos de origem locais atualizados é extraindo as alterações usando os comandos git.

Usando a linha de comando, defina o diretório atual para o repositório do Dynamo:

`cd C:\Users\username\Documents\GitHub\Dynamo`

> Substitua `"username"` por seu nome de usuário

Use o seguinte comando para extrair as alterações mais recentes:

`git pull origin master`

![Repositório local atualizado](images/cli-pull-changes.jpg)

> 1. Aqui podemos ver que o repositório local foi atualizado com as alterações do remoto.

Além de extrair as atualizações, há mais quatro fluxos de trabalho git com os quais se familiarizar.

* **Bifurcar** o repositório do Dynamo para criar uma cópia separada do original. Todas as alterações feitas aqui não afetarão o repositório original e as atualizações poderão ser buscadas ou enviadas com solicitações de extração. A bifurcação não é um comando git, mas é um fluxo de trabalho que o github adiciona – o modelo de solicitação de extração bifurcado é um dos fluxos de trabalho mais comuns para contribuir para projetos de código-fonte aberto on-line. Vale a pena aprender se você deseja contribuir no Dynamo.
* **Ramificação** – trabalhar em experimentos ou em novas operações isolados de outros trabalhos em ramificações. Isso facilita o envio de solicitações de extração.
* Fazer **confirmações** com frequência, após completar uma unidade de trabalho e após uma alteração que possa desejar ser desfeita. Os registros de confirmações mudam para o repositório e serão visíveis ao fazer uma solicitação de extração para o repositório principal do Dynamo.
* Criar **solicitações de extração** quando as alterações estiverem prontas para serem oficialmente propostas para o repositório principal do Dynamo.

A equipe do Dynamo tem instruções específicas sobre a criação de solicitações de extração. Consulte a seção Solicitações de extração nesta documentação para obter itens mais detalhados a serem tratados.

Consulte esta [página da documentação](https://git-scm.com/docs) para obter uma lista de referência de comandos git.
