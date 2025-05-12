# Configurar o Dynamo Player no Forma


Para usar o Dynamo com o Forma, existe duas opções: Dynamo como serviço (DaaS) na nuvem ou Dynamo no desktop. Cada um tem seus benefícios, dependendo do que você deseja fazer; portanto, antes de começar, considere qual opção melhor atende às suas necessidades. No entanto, lembre-se de que é possível alternar entre essas opções a qualquer momento.

**Comparação entre o Dynamo como serviço e o Dynamo Desktop**

<table><thead><tr><th>Dynamo como serviço</th><th>Desktop</th><th data-hidden></th></tr></thead><tbody><tr><td>Fácil configuração</td><td>Processo de instalação com várias etapas</td><td></td></tr><tr><td>Não é necessário instalar o Dynamo ou abri-lo</td><td>Tem que ter o Dynamo aberto</td><td></td></tr><tr><td>Não reconhece um gráfico que já está aberto no Dynamo</td><td>Abra um gráfico no Player que esteja aberto no Dynamo com um clique de botão</td><td></td></tr><tr><td>Não pode usar Python</td><td>Pode usar Python</td><td></td></tr><tr><td>Só pode usar pacotes sancionados</td><td>Pode usar qualquer pacote</td><td></td></tr></tbody></table>

Nesta página, explicaremos como configurar as duas opções.

### Configurar o Dynamo como um serviço

O Dynamo no Forma Beta está atualmente disponível como um beta aberto de acesso antecipado, o que significa que os recursos e a interface do usuário podem mudar com frequência.

Primeiro, vamos instalar o Dynamo Player no Forma.

1. No site do Forma, vá para **Extensions** (Extensões) na barra lateral esquerda e clique em **Add extension** (Adicionar extensão). Isso abre a Autodesk App Store.
2. Procure o Dynamo e adicione Dynamo Player Beta. Leia o aviso de isenção e clique em **Agree** (Concordo).

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Agora, o Dynamo Player está disponível nas extensões. Clique para abrir.
4. Agora você está pronto para usar o Dynamo Player.

### Configuração do Dynamo Desktop

Para usar o Dynamo Desktop, você precisará do Dynamo, como Sandbox independente ou conectado ao Revit ou ao Civil 3D. Você também precisará do pacote DynamoFormaBeta.

#### Revit

Siga estas instruções para configurar o Dynamo no Revit e no pacote DynamoFormaBeta.

1. Verifique se você tem o Revit 2024.1 ou superior instalado.
2. Abra o Dynamo no Revit acessando Gerenciar > Dynamo.
3. No Dynamo, instale o pacote DynamoFormaBeta. Vá para Pacotes > Gerenciador de pacotes e, em seguida, procure DynamoFormaBeta.
   1. Se você tiver o Revit 2024, instale o pacote DynamoFormaBeta para 2.x.
   2. Se você tiver o Revit 2025, instale o pacote DynamoFormaBeta.

#### Civil 3D

Siga estas instruções para configurar o Dynamo no Civil 3D e o pacote DynamoFormaBeta.

1. Verifique se você tem o Civil 3D 2024.1 ou superior instalado.
2. Abra o Dynamo no Civil 3D acessando Gerenciar > Dynamo.
3. No Dynamo, instale o pacote DynamoFormaBeta. Vá para Pacotes > Gerenciador de pacotes e, em seguida, procure DynamoFormaBeta.
   1. Se você tiver o Civil 3D 2024, instale o pacote DynamoFormaBeta para 2.x.
   2. Se você tiver o Civil 3D 2025, instale o pacote DynamoFormaBeta.

#### Dynamo Sandbox

Siga estas instruções para instalar o Dynamo Sandbox e o pacote DynamoFormaBeta.

1. Faça o download do Dynamo 2.18.0 ou superior em [Compilações do Dynamo](https://dynamobuilds.com/). Para ter a melhor experiência, escolha a mais recente das versões mais estáveis, listada na parte superior.
   1. As versões diárias são versões de desenvolvimento e podem incluir recursos incompletos ou em andamento.
2. Extraia o Dynamo usando [7zip](https://7zip.rnbastos.com/) para uma pasta de sua escolha.
3. Execute DynamoSandbox.exe na pasta de instalação do Dynamo.
4. No Dynamo, instale o pacote DynamoFormaBeta. Vá para Pacotes > Gerenciador de pacotes e, em seguida, procure DynamoFormaBeta.
   1. Se você tiver o Dynamo 2.x, instale o pacote DynamoFormaBeta para 2.x.
   2. Se você tiver o Dynamo 3.x, instale o pacote DynamoFormaBeta.

Quando Dynamo estiver instalado, você estará pronto para usá-lo com o Forma. Ao executar a opção de desktop do Dynamo no Forma, você precisará ter o Dynamo aberto para poder usar a extensão do Dynamo Player.

#### Acessar o Dynamo Desktop no Forma

Primeiro, vamos instalar o Dynamo Player no Forma.

1. No site do Forma, vá para **Extensions** (Extensões) na barra lateral esquerda e clique em **Add extension** (Adicionar extensão). Isso abre a Autodesk App Store.
2. Procure o Dynamo e adicione Dynamo Player Beta. Leia o aviso de isenção e clique em **Agree** (Concordo).

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Agora, o Dynamo Player está disponível nas extensões. Clique para abrir.
4. Perto da parte superior, clique em Desktop para acessar o Dynamo Desktop.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. Agora você está pronto para usar o Dynamo Player. Se já tiver um gráfico aberto no Dynamo, clique em Open (Abrir) em **Connected graph** (Gráfico conectado) para visualizá-lo no Player.
