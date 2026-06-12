# Dynamo Cloud Compute



O Dynamo Cloud Compute disponibiliza na nuvem o poder do runtime de programação visual do Dynamo. Em vez de executar os gráficos no computador local, o serviço de computação os executa em um ambiente na nuvem seguro e retorna os resultados.

## O que é o Dynamo?

O Dynamo é uma linguagem de programação visual e um ambiente de criação que permite criar programas conectando nós em um gráfico. O runtime do Dynamo executa esses gráficos, permitindo automatizar tarefas complexas, gerar geometria e integrar o Dynamo a outros softwares.

## Como funciona o serviço de computação

Quando você usa o Dynamo por meio de um cliente com base na nuvem (como o Dynamo Player no Forma), os arquivos de gráfico `.dyn` são enviados ao serviço de computação para execução. O serviço:

1. Recebe o gráfico e os parâmetros de entrada
2. Executa o gráfico em um ambiente na nuvem isolado
3. Retorna os resultados de volta para o aplicativo do cliente

Essa abordagem baseada em nuvem permite executar gráficos do Dynamo sem precisar instalar o Dynamo localmente, além de aproveitar o poder computacional da nuvem para operações complexas.

## Por que usar o Dynamo Cloud Compute?

O Dynamo Cloud Compute permite cenários em que você deseja:

**Executar gráficos sem a instalação no desktop**: execute gráficos do Dynamo diretamente em aplicativos da Web, sem que os usuários precisem instalar o Dynamo Desktop em seus computadores.

**Colaborar e compartilhar**: compartilhe gráficos com membros da equipe, que poderão executá-los por meio de interfaces da Web, como o Forma, facilitando a distribuição de fluxos de trabalho automatizados em toda a organização.

**Aproveitar a computação na nuvem**: aproveite a infraestrutura na nuvem para operações com uso intensivo de computação, que podem demorar mais em computadores locais.

**Padronizar o ambiente de execução**: garanta um comportamento consistente entre diferentes usuários e computadores executando gráficos em um ambiente na nuvem controlado.

**Conectar-se ao Forma**: use o Dynamo para interagir com a API do Forma. [Consulte esta postagem no blog para obter mais detalhes](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/).

## Características principais

**Execução na nuvem**: os gráficos são executados na nuvem, e não no computador local. Isso significa:
- Não é necessário instalar o Dynamo Desktop para executar os gráficos
- Acesso a recursos de computação na nuvem
- Ambiente de execução consistente entre diferentes usuários

**Segurança**: o serviço executa os gráficos de cada usuário em ambientes isolados para garantir a segurança e a privacidade dos dados. Os gráficos e os dados são mantidos separados dos de outros usuários.

**Processamento assíncrono**: a execução do gráfico ocorre de forma assíncrona: os clientes enviam um trabalho e podem verificar seu status até que ele seja concluído. Isso permite cálculos de longa duração sem bloquear o fluxo de trabalho.

## Disponibilidade atual

O Dynamo Cloud Compute está atualmente disponível por meio do:
- **Reprodutor do Dynamo no Forma (Beta aberto)**: carregue, compartilhe e execute gráficos do Dynamo diretamente na interface da Web do Autodesk Forma.

## Saiba mais

- [Diferenças entre o Dynamo Cloud Compute e o Dynamo Desktop](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md) – Diferenças importantes a serem consideradas ao criar gráficos para execução na nuvem
- [Ciclo de vida do mecanismo](engine-lifecycle.md) – Informações sobre as versões suportadas do mecanismo e seus ciclo de vida

-----


> **Observação: Serviço beta**  
 O Dynamo Cloud Compute está atualmente em versão beta. Os cronogramas de suporte e as políticas de atualização descritos neste documento representam nossas intenções atuais, à medida que experimentamos e aprimoramos o serviço. Essas informações não constituem garantias e podem ser alteradas com base no feedback dos usuários e na experiência operacional.