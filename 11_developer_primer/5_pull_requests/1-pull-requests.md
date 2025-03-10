# Solicitações de extração

O Dynamo depende da criatividade e do compromisso de sua comunidade, e a equipe do Dynamo incentiva os colaboradores a explorar possibilidades, testar ideias e envolver a comunidade para enviar feedback. Embora a inovação seja altamente encorajada, as alterações só serão mescladas se facilitarem o uso do Dynamo e satisfizerem as diretrizes definidas neste documento.  As alterações com benefícios pouco definidos não serão incorporadas.

#### Expectativas das solicitações de extração <a href="#pull-request-expectations" id="pull-request-expectations"></a>

A equipe do Dynamo espera que as solicitações de extração sigam algumas diretrizes:

* Siga nossos [padrões de codificação](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) e [padrões de nomenclatura de nós](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* Inclua testes de unidade durante a adição de novos recursos
* Ao corrigir um erro, adicione um teste de unidade que realce como o comportamento atual é quebrado
* Mantenha a discussão focada em um problema. Crie um novo problema se um tópico novo ou relacionado for exibido.

E algumas diretrizes sobre o que não fazer:

* Surpreender-nos com grandes pedidos de extração. Em vez disso, registre um problema e inicie uma discussão para que possamos concordar sobre a abordagem a ser seguida antes de investir um grande tempo.
* Confirmar o código que você não escreveu. Se você encontrar um código que você acha que é uma boa opção para adicionar ao Dynamo, registre um problema e inicie uma discussão antes de continuar.
* Enviar representações posicionais que alteram os arquivos ou cabeçalhos relacionados ao licenciamento. Se você achar que há um problema com eles, envie um problema e ficaremos felizes em discuti-lo.
* Fazer adições de API sem apresentar um problema e discuti-lo conosco primeiro.

#### Preencher o modelo de solicitação de extração <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Ao enviar uma solicitação de extração, use o [modelo PR padrão](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Antes de enviar a representação posicional, certifique-se de que o propósito esteja claramente descrito e que todas as declarações possam ser afirmadas como verdadeiras:

* A base de código está em um estado melhor após esta representação posicional
* É documentado de acordo com as [normas](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)
* O nível de teste que esta representação posicional inclui é apropriado
* Sequências de caracteres voltadas para o usuário, se existirem, são extraídas para arquivos `*.resx`
* Todos os testes passam usando o CI de autoatendimento
* Instantâneo das alterações da interface do usuário, se houver
* As alterações na API seguem a [Versão semântica](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) e são documentadas no documento [Alterações da API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

Um revisor apropriado será atribuído à solicitação de extração pela equipe do Dynamo.

#### Processo de revisão das solicitações de extração <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Depois de uma solicitação de extração ser enviada, talvez seja necessário permanecer envolvido durante o processo de revisão. Esteja ciente dos seguintes critérios de revisão:

* A equipe do Dynamo se reúne uma vez por mês para analisar as solicitações de extração das mais antigas para as mais recentes.
* Se uma solicitação de extração revisada exigir alterações pelo proprietário, o proprietário da representação posicional terá 30 dias para responder. Se a representação posicional não tiver registrado nenhuma atividade na próxima sessão, ela será fechada pela equipe ou, dependendo de sua utilidade, será assumida por alguém da equipe.
* As solicitações de extração devem usar o modelo de RP padrão do Dynamo
* As solicitações de extração que não têm os modelos de RP do Dynamo completamente preenchidos com todas as declarações satisfeitas não serão revisadas.

#### Escolha seletiva de confirmações do Dynamo Revit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Como há várias versões do Revit disponíveis no mercado, talvez seja necessário escolher cuidadosamente as alterações em ramificações específicas da versão do DynamoRevit para que diferentes versões do Revit possam obter a nova funcionalidade. Durante o processo de revisão, os colaboradores serão responsáveis por selecionar cuidadosamente suas confirmações revisadas para as outras ramificações do DynamoRevit, conforme especificado pela equipe do Dynamo.
