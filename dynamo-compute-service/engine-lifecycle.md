# Ciclo de vida e frequência de atualização do Dynamo Compute Service



Este documento descreve a frequência de atualização e a política de suporte do Dynamo Cloud Compute. Também pode ser referido neste documento de forma intercambiável como “o serviço”.

Ele descreve como as versões do mecanismo são gerenciadas, quando ocorrem atualizações e o que os usuários podem esperar ao executar gráficos do Dynamo na nuvem.

---

## Frequência de atualização

Para atender a diferentes necessidades dos usuários, o Dynamo Cloud Compute mantém **duas instâncias distintas do mecanismo**. Cada mecanismo atende a uma finalidade específica e segue sua própria frequência de atualização:

### Mecanismo estável (Produção)

O mecanismo estável foi projetado para oferecer confiabilidade e consistência em ambientes de produção. Ele se baseia na versão estável mais recente do DynamoCore Runtime e será atualizado quando as versões oficiais do Dynamo estiverem acessíveis aos usuários de desktop do Dynamo. Inicialmente, seguiremos a frequência de atualização do DynamoRevit.

Essa instância destina-se a cargas de trabalho de produção em que a confiabilidade e a previsibilidade são essenciais. Quando você usa o mecanismo estável, pode esperar que as atualizações acompanhem o cronograma de lançamento público do Dynamo, dando-lhe tempo para se preparar para as alterações e testar os gráficos antes que eles afetem os fluxos de trabalho.

### Mecanismo de visualização (Visualização/Sandbox diária)

O mecanismo de visualização fornece acesso antecipado aos desenvolvimentos mais recentes do Dynamo. Ele se baseia na versão de desenvolvimento mais recente do DynamoCore Runtime e é atualizado com frequência à medida que novos recursos e correções de bugs são incorporados.

Essa instância é ideal para usuários que desejam testar as próximas alterações, experimentar novos recursos antes de serem lançados oficialmente ou verificar se seus gráficos continuarão funcionando com versões futuras do Dynamo. O mecanismo de visualização permite que você fique à frente das alterações e forneça feedback à equipe do Dynamo.

---


## Cronograma de suporte

Entender por quanto tempo cada versão do mecanismo permanece com suporte ajuda você a planejar janelas de manutenção e a atualizações de gráficos.

### Mecanismo estável

O mecanismo estável recebe atualizações quando o Dynamo publica uma nova versão estável do Dynamo Core no Revit. Cada versão estável permanece disponível e com suporte até que a próxima versão estável seja implantada no serviço.

Por exemplo, se o serviço estiver executando atualmente o Dynamo 3.6 (estável), ele continuará usando essa versão até que o Dynamo 4.0 seja disponibilizado oficialmente para todos os usuários (normalmente quando for lançado no Revit). Nesse ponto, o serviço será atualizado para o Dynamo 4.0 (estável).

Essa abordagem garante que o serviço na nuvem permaneça sincronizado com o que a maioria dos usuários experimenta em ambientes de desktop.

### Mecanismo de visualização

O mecanismo de visualização é atualizado continuamente na ramificação de desenvolvimento mais recente do Dynamo. À medida que o desenvolvimento avança em cada versão, o mecanismo de visualização rastreia essas alterações.

Por exemplo, enquanto o Dynamo 4.1 está em desenvolvimento ativo, o mecanismo de visualização pode ser rotulado como “Dynamo Cloud Compute Service 4.1”. Quando o desenvolvimento mudar para a versão 4.2, o mecanismo de visualização começará a rastrear essas alterações e poderá ser renomeado como “Dynamo Cloud Compute Service 4.2”.

Como o mecanismo de visualização é atualizado com frequência, você deve esperar alterações ocasionais que podem causar incompatibilidades ou recursos experimentais. É mais adequado para testes e validação, em vez de fluxos de trabalho de produção.

---

## Como escolher o mecanismo certo

Ao decidir qual mecanismo usar:

- **Escolha estável** se você precisar de um comportamento previsível e testado para fluxos de trabalho de produção ou se estiver implantando gráficos para usuários finais que esperam resultados consistentes.

- **Escolha visualização** se desejar testar novos recursos antecipadamente, validar se os gráficos funcionarão com versões futuras ou contribuir com feedback para o desenvolvimento do Dynamo.

Ambos os mecanismos usam o mesmo ambiente de execução principal do Dynamo. A diferença está no momento e na frequência com que recebem atualizações. 

---

> **Observação: Serviço beta**  
 O Dynamo Cloud Compute está atualmente em versão beta. Os cronogramas de suporte e as políticas de atualização descritos neste documento representam nossas intenções atuais, à medida que experimentamos e aprimoramos o serviço. Essas informações não constituem garantias e podem ser alteradas com base no feedback dos usuários e na experiência operacional.



