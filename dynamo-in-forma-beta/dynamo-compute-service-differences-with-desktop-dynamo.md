# O Dynamo calcula diferenças de serviço com o Dynamo Desktop

Esta página destaca as diferenças sobre as quais você deve estar ciente ao escrever programas do Dynamo para executar no contexto de nuvem de serviço de cálculo do Dynamo.

## O que é o DaaS?

DaaS, Dynamo como serviço, serviço de cálculo do Dynamo etc. referem-se à mesma coisa: o tempo de execução principal do Dynamo em execução em um contexto de nuvem. Isso significa que o gráfico não está sendo executado no computador. Atualmente, só é possível acessar o DaaS por meio da extensão do Dynamo Player para Forma, na qual os usuários podem carregar e gerenciar arquivos `.dyn` criados no ambiente de desktop, executar arquivos `.dyn` compartilhados por seus colegas por meio da extensão ou usar rotinas `.dyn` pré-carregadas fornecidas pela Autodesk como exemplos.

Como os gráficos são executados nesse contexto de nuvem, e não no computador, o DaaS atualmente não pode usar diretamente contextos de hospedeiros tradicionais do Dynamo (Revit, Civil 3D etc.). Se você desejar usar tipos desses programas no gráfico, será necessário serializá-los (salvá-los) no gráfico usando o nó `Data.Remember` ou outras técnicas de serialização no gráfico. Esses fluxos de trabalho são semelhantes aos fluxos de trabalho que você precisa usar ao desenhar gráficos para o Projeto generativo no Revit.

## Qual versão do Dynamo está executando meu código?

A versão é baseada na versão 3.x e é atualizada frequentemente com base na ramificação mestre de código aberto do Dynamo.

## Quais pacotes/nós estão disponíveis nesta versão do Dynamo?

* A maioria dos nós principais. Consulte a próxima seção para obter algumas limitações específicas.
* Pacote `DynamoFormaBeta` para interagir com a API do Forma.
* `VASA` para voxelização/análise eficiente.
* `MeshToolKit` para manipulação de malha. O kit de ferramentas de malha também está disponível pronto para uso a partir do Dynamo 3.4.
* `RefineryToolkit` para algoritmos úteis que permitem o teste de interferência, distância de visualização, caminho mais curto, isovista etc.

## O que devo saber ao escrever gráficos para o DaaS?

* Os nós do Python não funcionam. _No momento_, eles simplesmente não são executados.
* Não é possível usar pacotes personalizados.
* A camada de interface do usuário/vista dos nós da interface do usuário não será executada. Não prevemos que isso seja um problema com a funcionalidade principal, mas é bom ter em mente se você vir uma falha relacionada a um nó com a interface do usuário personalizada.
* A funcionalidade exclusiva do Windows não funcionará. Por exemplo, se você tentar usar o Registro do Windows ou WPF, ocorrerá uma falha.
* As extensões da vista não serão carregadas.
* Os nós do sistema de arquivos não serão muito úteis. Os arquivos que você referenciar no computador local não existirão quando executados no DaaS.
* Os nós de interoperabilidade do Excel/DSOffice não funcionarão. Os nós do Open XML deverão funcionar.
* As solicitações de rede em geral não funcionarão, embora você possa fazer chamadas para a API do Forma.

## Como vou me lembrar de tudo isso? E se isso mudar?

* No futuro, pretendemos fornecer ferramentas dentro do Dynamo para desktop que tornarão mais fácil garantir que o gráfico seja executado da mesma forma em ambos os contextos.

## Quanto isso custa?

* Durante esta versão Beta, o tempo de cálculo não é cobrado no momento.

## Como faço para começar?

Confira a [postagem do blog](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), a [série do YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) ou os Exemplos na extensão do Forma para começar. Eles guiarão você para:

* Obter acesso ao Autodesk Forma.
* Instalar o DynamoFormaBeta para Dynamo em desktop e a extensão do Dynamo no Forma.
* Escrever o primeiro gráfico.

## Segurança

* Lembre-se de que os gráficos compartilhados são armazenados no Forma.
* Atualmente, o tempo máximo de execução do gráfico é inferior a 30 minutos. Esse valor pode mudar.
* As solicitações de execução são limitadas em taxas; portanto, você poderá ver erros se fizer muitas solicitações de cálculo em um período muito curto.
