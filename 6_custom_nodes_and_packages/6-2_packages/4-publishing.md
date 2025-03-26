# Publicar um pacote

Nas seções anteriores, analisamos os detalhes de como o pacote _MapToSurface_ é configurado com nós personalizados e arquivos de exemplo. Mas como publicamos um pacote que foi desenvolvido localmente? Este estudo de caso demonstra como publicar um pacote de um conjunto de arquivos em uma pasta local.

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Há diversas formas de publicar um pacote. Confira abaixo o processo que aconselhamos: **publicar localmente, desenvolver localmente e publicar on-line**. Vamos começar com uma pasta contendo todos os arquivos do pacote.

### Desinstalar um pacote

Antes de passarmos para a publicação do pacote MapToSurface, se você instalou o pacote da lição anterior, desinstale-o para que não esteja trabalhando com pacotes idênticos.

Para começar, vá para a guia Pacotes > Package Manager > Pacotes instalados > ao lado de MapToSurface, clique no menu de pontos verticais > Excluir.

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

Em seguida, reinicie o Dynamo. Depois de reabrir, quando você verificar a janela _“Gerenciar pacotes”_, o _MapToSurface_ não deverá mais estar lá. Agora estamos prontos para começar.

### Publicar um pacote localmente

{% hint style="warning" %} É possível publicar nós e pacotes personalizados do Dynamo Sandbox na versão 2.17 e mais recentes, desde que não tenham dependências de APIs do hospedeiro. Em versões anteriores, a publicação de nós e pacotes personalizados somente estava ativada no Dynamo for Revit e no Dynamo for Civil 3D. {% endhint %}

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Este é o primeiro envio para o nosso pacote e colocamos todos os arquivos de exemplo e nós personalizados em uma pasta. Com essa pasta preparada, estamos prontos para carregar no Gerenciador de pacotes do Dynamo.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Essa pasta contém cinco nós personalizados (.dyf).
> 2. Essa pasta também contém cinco arquivos de exemplo (.dyn) e um arquivo de vetor importado (.svg). Esses arquivos servirão como exercícios introdutórios para mostrar ao usuário como trabalhar com os nós personalizados.

No Dynamo, comece clicando na guia _Pacotes > Package Manager > Publicar novo pacote_.

Na guia _Publicar um pacote_, preencha os campos relevantes no lado esquerdo da janela.

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

Em seguida, adicionaremos arquivos de pacote. É possível adicionar arquivos um a um ou todas as pastas selecionando Adicionar diretório (1). Para adicionar arquivos que não são arquivos .dyf, assegure-se de alterar o tipo de arquivo na janela do navegador para **“Todos os arquivos (**_._**)”**. Observe que vamos adicionar indiscriminadamente cada arquivo, nó personalizado (.dyf) ou arquivo de exemplo (.dyn). O Dynamo categorizará esses itens quando o pacote for publicado.

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

Depois de selecionar a pasta MapToSurface, o Package Manager mostrará o conteúdo da pasta. Se você estiver carregando seu próprio pacote com uma estrutura de pastas complexa e não quiser que o Dynamo faça alterações na estrutura de pastas, poderá ativar a alternância “Manter estrutura de pastas”. Essa opção é para usuários avançados e, se o pacote não for configurado deliberadamente de uma forma específica, será melhor deixar essa alternância desativada e permitir que o Dynamo organize os arquivos conforme necessário. Clique em Avançar para continuar.

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

Aqui, você tem a chance de visualizar como o Dynamo organizará os arquivos do pacote antes da publicação. Clique em Concluir para continuar.

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

Publique clicando em “Publicar localmente” (1). Após completar esses passos, certifique-se de clicar em _“Publicar localmente”_ e **não** em _“Publicar on-line”_ para evitar ter um acúmulo de pacotes duplicados no Package Manager.

Após a publicação, os nós personalizados devem estar disponíveis no grupo “DynamoPrimer” ou na Biblioteca do Dynamo.

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Agora, vamos examinar o diretório raiz para ver como o Dynamo formatou o pacote que acabamos de criar. Para fazer isso, vá para a guia Pacotes instalados > ao lado de MapToSurface, clique no menu de pontos verticais > selecione Mostrar diretório raiz.

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

Observe que o diretório raiz está na localização local do pacote (lembre-se de que publicamos o pacote “localmente”). O Dynamo está fazendo referência a esta pasta para ler nós personalizados. Portanto, é importante publicar localmente o diretório em uma localização de pasta permanente (ou seja, não na área de trabalho). Veja a seguir o detalhamento da pasta do pacote do Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. A pasta _bin_ contém os arquivos .dll criados com bibliotecas C# ou Sem toque. Não temos nenhum arquivo desse tipo nesse pacote; portanto, a pasta está em branco neste exemplo.
> 2. A pasta _dyf_ contém os nós personalizados. Depois de abrir essa pasta, serão exibidos todos os nós personalizados (arquivos .dyf) desse pacote.
> 3. A pasta adicional contém todos os arquivos adicionais. É provável que esses arquivos sejam arquivos do Dynamo (.dyn) ou qualquer arquivo adicional necessário (.svg, .xls, .jpeg, .sat, etc.).
> 4. O arquivo pkg é um arquivo de texto básico que define as configurações do pacote. Isso é automatizado no Dynamo, mas pode ser editado se você deseja entrar nos detalhes.

### Publicar um pacote on-line

{% hint style="warning" %} Observação: Siga estas etapas somente se você estiver realmente publicando um pacote próprio. {% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. Quando estiver pronto para publicar, na janela Pacotes > Package Manager > Pacotes instalados, selecione o botão à direita do pacote que deseja publicar e escolha Publicar.
2. Se você estiver atualizando um pacote que já foi publicado, selecione “Publicar versão” e o Dynamo atualizará o pacote on-line com base nos novos arquivos no diretório raiz do pacote. É tão simples quanto isso.

#### Testar o servidor do gerenciador de pacotes
Ao testar o gerenciador de pacotes, não envie os pacotes de teste para o servidor de produção. Use o servidor de preparação. Isso evita que os pacotes poluam pacotes e atividades reais. É fácil configurar o Dynamo para usar o servidor de preparação. 

Para obter mais informações sobre isso, consulte a [página wiki Testar o servidor do gerenciador de pacotes](https://github.com/DynamoDS/Dynamo/wiki/Testing-the-Package-Manager-Server).

### Publicar versão...

Ao atualizar os arquivos na pasta raiz do pacote publicado, também é possível publicar uma nova versão do pacote selecionando _“Publicar versão...”_ na guia _Meus pacotes_. Essa é uma forma perfeita para fazer atualizações necessárias ao conteúdo e para compartilhar com a comunidade. A opção _Publicar versão_ só funcionará se você for o administrador do pacote.
