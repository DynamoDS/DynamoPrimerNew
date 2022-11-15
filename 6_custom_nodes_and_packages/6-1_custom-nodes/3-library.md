# Publicar na biblioteca

Acabamos de criar um nó personalizado e aplicá-lo a um processo específico em nosso gráfico do Dynamo. Gostamos tanto desse nó, por isso queremos mantê-lo em nossa biblioteca do Dynamo para usar como referência em outros gráficos. Para fazer isso, vamos publicar o nó localmente. Este é um processo semelhante ao de publicar um pacote, que iremos mostrar em mais detalhes no próximo capítulo.

Quando publicar um nó localmente, o nó estará acessível na biblioteca do Dynamo quando você abrir uma nova sessão. Se nenhum nó for publicado, o gráfico do Dynamo que faz referência a um nó personalizado também deverá ter aquele nó personalizado em sua pasta (ou o nó personalizado deverá ser importado para o Dynamo usando _Arquivo>Importar biblioteca_).

{% hint style="warning" %} A publicação de nó personalizado está ativada somente no Dynamo for Revit e no Dynamo for Civil 3D. O Dynamo Sandbox não tem a funcionalidade de publicação. {% endhint %}

## Exercício: Publicar um nó personalizado localmente

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

Vamos avançar com o nó personalizado que criamos na seção anterior. Quando o nó personalizado PointsToSurface é aberto, vemos o gráfico no Editor de nós personalizados do Dynamo. Também é possível abrir um nó personalizado clicando duas vezes nele no Editor de gráficos do Dynamo.

![](../images/6-1/3/publishcustomnodelocally01.jpg)

Para publicar um nó personalizado localmente, basta clicar com o botão direito do mouse na tela e selecionar _“Publicar este nó personalizado...”_

![](../images/6-1/3/publishcustomnodeexercise-02.jpg)

Preencha as informações relevantes de forma similar à imagem acima e selecione _“Publicar localmente”_. Observe que o campo Grupo define o elemento principal acessível no menu do Dynamo.

![](../images/6-1/3/publishcustomnodeexercise-03.jpg)

Escolha uma pasta para armazenar todos os nós personalizados que você planeja publicar localmente. O Dynamo verificará essa pasta sempre que ela for carregada, portanto, certifique-se de que a pasta esteja em um local permanente. Navegue até essa pasta e escolha _“Selecionar pasta”_. Agora, o nó do Dynamo está publicado localmente e permanecerá na barra de ferramentas do Dynamo cada vez que o programa for carregado.

![](../images/6-1/3/publishcustomnodeexercise-04.jpg)

Para verificar o local da pasta do nó personalizado, vá para _Dynamo > Preferências> Gerenciador de pacotes > Caminhos de nó e pacote_

![](../images/6-1/3/publishcustomnodeexercise-05.jpg)

Nessa janela, vemos uma lista de caminhos.

![](../images/6-1/3/publishcustomnodeexercise-06.jpg)

> 1. _Documentos\\DynamoCustomNodes..._ faz referência à localização dos nós personalizados que publicamos localmente.
> 2. _AppData\\Roaming\\Dynamo..._ refere-se à localização padrão dos pacotes do Dynamo instalados on-line.
> 3. Você pode querer mover o caminho da pasta local para baixo na ordem da lista acima (selecionando o caminho da pasta e clicando na seta para baixo à esquerda dos nomes do caminho). A pasta superior é o caminho padrão para a instalação do pacote. Portanto, mantendo o caminho de instalação do pacote do Dynamo padrão como a pasta padrão, os pacotes on-line serão separados dos nós publicados localmente.

Alteramos a ordem dos nomes de caminhos para que o caminho padrão do Dynamo seja o local de instalação do pacote.

![](../images/6-1/3/publishcustomnodeexercise-07.jpg)

Navegando para essa pasta local, poderemos encontrar o nó personalizado original na pasta _“.dyf”_, que é a extensão de arquivo de nós personalizados do Dynamo. Podemos editar o arquivo nessa pasta e o nó será atualizado na interface do usuário. Também é possível adicionar mais nós à pasta principal _DynamoCustomNode_ e o Dynamo os adicionará à biblioteca ao ser reiniciado.

![](../images/6-1/3/publishcustomnodeexercise-08.jpg)

O Dynamo agora será carregado sempre com “PointsToSurface” no grupo “DynamoPrimer” da biblioteca do Dynamo.

![](../images/6-1/3/publishcustomnodeexercise-09.jpg)
