# Publicar um pacote

Nas seções anteriores, analisamos os detalhes de como o pacote _MapToSurface_ é configurado com nós personalizados e arquivos de exemplo. Mas como publicamos um pacote que foi desenvolvido localmente? Este estudo de caso demonstra como publicar um pacote de um conjunto de arquivos em uma pasta local.

![](../images/6-2/4/publishapackage-customnodes01.jpg)

Há diversas formas de publicar um pacote. Confira abaixo o processo que aconselhamos: **publicar localmente, desenvolver localmente e publicar on-line**. Vamos começar com uma pasta contendo todos os arquivos do pacote.

### Desinstalar um pacote

Antes de passarmos para a publicação do pacote MapToSurface, se você instalou o pacote da lição anterior, desinstale-o para que não esteja trabalhando com pacotes idênticos.

Comece navegando para Dynamo > Preferências > Gerenciador de pacotes > ao lado de MapToSurface, clique no menu de pontos verticais > excluir

![](../images/6-2/4/publishapackage-deletepackage.jpg)

Em seguida, reinicie o Dynamo. Depois de reabrir, quando você verificar a janela _“Gerenciar pacotes”_, o _MapToSurface_ não deverá mais estar lá. Agora estamos prontos para começar.

### Publicar um pacote localmente

{% hint style="warning" %} A publicação de pacote do Dynamo está ativada somente no Dynamo for Revit e no Dynamo for Civil 3D. O Dynamo Sandbox não tem a funcionalidade de publicação. {% endhint %}

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Este é o primeiro envio para o nosso pacote e colocamos todos os arquivos de exemplo e nós personalizados em uma pasta. Com essa pasta preparada, estamos prontos para carregar no Gerenciador de pacotes do Dynamo.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Essa pasta contém cinco nós personalizados (.dyf).
> 2. Essa pasta também contém cinco arquivos de exemplo (.dyn) e um arquivo de vetor importado (.svg). Esses arquivos servirão como exercícios introdutórios para mostrar ao usuário como trabalhar com os nós personalizados.

No Dynamo, comece clicando em _Pacotes>Publicar novo pacote..._

![](../images/6-2/4/publishapackage-publishlocally02.jpg)

Na janela _“Publicar um pacote do Dynamo”_, preencha os formulários relevantes à esquerda da janela.

![](../images/6-2/4/publishapackage-publishlocally03.jpg)

> 1. Clicando em _“Adicionar arquivo”_, também adicionamos os arquivos da estrutura de pastas no lado direito da tela (para adicionar arquivos que não são arquivos .dyf, certifique-se de alterar o tipo de arquivo na janela do navegador para **“Todos os arquivos (**_**.**_**)”**. Observe que adicionamos cada arquivo, nó personalizado (.dyf) ou arquivo de exemplo (.dyn) de forma indiferenciada. O Dynamo classificará esses itens em categorias quando publicarmos o pacote.
> 2. O campo "Grupo" define qual grupo localizará os nós personalizados na interface do usuário do Dynamo.
> 3. Publique clicando em “Publicar localmente”. Após completar esses passos, certifique-se de clicar em _“Publicar localmente”_ e **não** em _“Publicar on-line”_; é recomendável evitar o acúmulo de pacotes duplicados no Gerenciador de pacotes.

Após a publicação, os nós personalizados devem estar disponíveis no grupo “DynamoPrimer” ou na Biblioteca do Dynamo.

![](../images/6-2/4/publishapackage-publishlocally04.jpg)

Agora, vamos examinar o diretório raiz para ver como o Dynamo formatou o pacote que acabamos de criar. Para fazer isso, clique em Dynamo > Preferências > Gerenciador de pacotes > ao lado de MapToSurface, clique no menu de pontos verticais > selecione Mostrar diretório raiz

![](../images/6-2/4/publishapackage-publishlocally05.jpg)

Observe que o diretório raiz está na localização local do pacote (lembre-se de que publicamos o pacote “localmente”). O Dynamo está fazendo referência a esta pasta para ler nós personalizados. Portanto, é importante publicar localmente o diretório em um local de pasta permanente (isto é, não em seu desktop). Veja a seguir o detalhamento da pasta do pacote do Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. A pasta _bin_ contém os arquivos .dll criados com bibliotecas C# ou Sem toque. Não temos nenhum arquivo desse tipo nesse pacote; portanto, a pasta está em branco neste exemplo.
> 2. A pasta _dyf_ contém os nós personalizados. Depois de abrir essa pasta, serão exibidos todos os nós personalizados (arquivos .dyf) desse pacote.
> 3. A pasta adicional contém todos os arquivos adicionais. É provável que esses arquivos sejam arquivos do Dynamo (.dyn) ou qualquer arquivo adicional necessário (.svg, .xls, .jpeg, .sat, etc.).
> 4. O arquivo pkg é um arquivo de texto básico que define as configurações do pacote. Isso é automatizado no Dynamo, mas pode ser editado se você deseja entrar nos detalhes.

### Publicar um pacote on-line

{% hint style="warning" %} Observação: Siga estas etapas somente se você estiver realmente publicando um pacote próprio. {% endhint %}

![](../images/6-2/4/publishapackage-publishonline01.jpg)

> 1. Quando estiver pronto para publicar, na janela Preferências > Gerenciador de pacotes, selecione o botão à direita de MapToSurface e escolha _Publicar..._
> 2. Se você estiver atualizando um pacote que já foi publicado, selecione “Publicar versão” e o Dynamo atualizará o pacote on-line com base nos novos arquivos no diretório raiz do pacote. Simples assim!

### Publicar versão...

Quando você atualiza os arquivos na pasta raiz do pacote publicado, é possível publicar uma nova versão do pacote selecionando _“Publicar versão...”_ na janela _Gerenciar pacotes_. Essa é uma forma perfeita para fazer atualizações necessárias ao conteúdo e para compartilhar com a comunidade. A opção _Publicar versão_ só funcionará se você for o administrador do pacote.
