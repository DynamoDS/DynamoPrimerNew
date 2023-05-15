# Publicar um pacote

### Publicar um pacote <a href="#publish-a-package" id="publish-a-package"></a>

Os pacotes são uma forma conveniente de armazenar e compartilhar nós com a comunidade do Dynamo. Um pacote pode conter tudo, desde nós personalizados criados no espaço de trabalho do Dynamo até nós derivados do NodeModel. Os pacotes são publicados e instalados usando o Gerenciador de pacotes. Além desta página, o [Manual](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) tem um guia geral sobre os pacotes.

#### O que é um Gerenciador de pacotes? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

O Gerenciador de pacotes do Dynamo é um Registro de software (semelhante ao npm) que pode ser acessado no Dynamo ou em um navegador da Web. O Gerenciador de pacotes inclui a instalação, publicação, atualização e visualização de pacotes. Como o npm, ele mantém diferentes versões de pacotes. Também ajuda a gerenciar as dependências do projeto.

No navegador, procure pacotes e visualize as estatísticas: [https://dynamopackages.com/](https://dynamopackages.com)

* No Dynamo, o Gerenciador de pacotes inclui pacotes de instalação, publicação e atualização.

![Procurar pacotes](images/dynamopackagemanager.jpg)

> 1. Procurar pacotes on-line: `Packages > Search for a Package...`
> 2. Visualizar/editar pacotes instalados: `Packages > Manage Packages...`
> 3. Publicar um novo pacote: `Packages > Publish New Package...`

#### Publicar um pacote <a href="#publishing-a-package" id="publishing-a-package"></a>

Os pacotes são publicados do Gerenciador de pacotes no Dynamo. O processo recomendado é publicar localmente, testar o pacote e, em seguida, publicar on-line para compartilhar com a comunidade. Usando o estudo de caso NodeModel, vamos passar pelas etapas necessárias para publicar o nó RetangularGrid como um pacote localmente e, em seguida, on-line.

Inicie o Dynamo e selecione `Packages > Publish New Package...` para abrir a janela `Publish a Package`.

![Publicar um pacote](images/dyn-publish-package-add-files.jpg)

> 1. Selecionar `Add file...` para procurar arquivos para adicionar ao pacote
> 2. Selecionar os dois arquivos `.dll` no estudo de caso NodeModel
> 3. Selecionar `Ok`

Com os arquivos adicionados ao conteúdo do pacote, atribua um nome, uma descrição e uma versão ao pacote. A publicação de um pacote usando o Dynamo cria automaticamente um arquivo `pkg.json`.

![Configurações do pacote](images/dyn-publish-package.jpg)

> Um pacote pronto para ser publicado.
>
> 1. Fornecer as informações necessárias para o nome, a descrição e a versão.
> 2. Publicar clicando em “Publicar localmente” e selecionar a pasta de pacotes do Dynamo: `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` para ter o nó disponível no Core. Sempre publique localmente até que o pacote esteja pronto para compartilhar.

Após a publicação de um pacote, os nós estarão disponíveis na biblioteca do Dynamo na categoria `CustomNodeModel`.

![Pacote na biblioteca do Dynamo](images/dyn-publish-package-library.jpg)

> 1. O pacote que acabamos de criar na biblioteca do Dynamo

Quando o pacote estiver pronto para publicação on-line, abra o Gerenciador de pacotes, escolha `Publish` e, em seguida, `Publish Online`.

![Publicar um pacote no Gerenciador de pacotes](images/dyn-publish-package-directory.jpg)

> 1. Para ver como o Dynamo formatou o pacote, clicar nos três pontos verticais à direita de “CustomNodeModel” e escolher “Mostrar diretório raiz”
> 2. Selecionar `Publish` e, em seguida, `Publish Online` na janela “Publicar um pacote do Dynamo”.
> 3. Para excluir um pacote, selecionar `Delete`.

#### Como atualizar um pacote? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

A atualização de um pacote é um processo semelhante ao da publicação. Abra o Gerenciador de pacotes e selecione `Publish Version...` no pacote que precisa ser atualizado e insira uma versão posterior.

![Publicar uma versão do pacote](images/dyn-publish-package-version.jpg)

> 1. Selecionar `Publish Version` para atualizar um pacote existente com novos arquivos no diretório raiz e, em seguida, escolher se ele deve ser publicado localmente ou on-line.

#### Cliente Web do Gerenciador de pacotes <a href="#package-manager-web-client" id="package-manager-web-client"></a>

O cliente Web do Gerenciador de pacotes é usado exclusivamente para pesquisar e visualizar dados de pacotes, como o controle de versão e estatísticas de download.

É possível acessar o cliente Web do Gerenciador de pacotes neste link: [https://dynamopackages.com/](https://dynamopackages.com)

![Cliente Web do gerenciador de pacotes](images/packagemanager-browser.jpg)
