# O que é o servidor MCP do Dynamo

**MCP** significa Model Context Protocol (Protocolo de Contexto de Modelo). É um padrão aberto que permite que modelos de IA se conectem ao software por meio de um protocolo comum. Um servidor MCP disponibiliza as ferramentas e os dados de um programa para que um cliente de IA possa ler o contexto em tempo real, executar ações e retornar resultados estruturados.

O **DynamoMCP** é o servidor MCP integrado do Dynamo. Ele conecta o Autodesk Assistant à sessão ativa do Dynamo (gráfico, nós, conexões e estado do espaço de trabalho) para que a ajuda da IA seja baseada naquilo em que você está realmente trabalhando.

Para quem usa o Dynamo, isso significa que o Assistant pode fazer mais do que responder a perguntas gerais. Ele pode inspecionar o gráfico aberto, ajudar a solucionar problemas com nós e atualizar a tela com base no contexto real da sessão, em vez de copiar e colar manualmente.

O DynamoMCP está disponível no Dynamo 4.2.0 e versões posteriores.

### Como usar o DynamoMCP

Você usa o DynamoMCP por meio do Autodesk Assistant; não há configuração separada nem interface independente. O Assistant é o ponto de entrada para fazer perguntas, obter ajuda com o gráfico e permitir que a IA trabalhe com o contexto ativo do Dynamo.

O local onde você abre o Assistant depende do ambiente em que está trabalhando:

- **Dynamo Sandbox** — as ferramentas do DynamoMCP estão disponíveis no Autodesk Assistant dentro do Dynamo.
- **Revit** — as ferramentas do DynamoMCP estão disponíveis no Autodesk Assistant dentro do Revit.

Em ambos os casos, você permanece na sessão atual e usa a mesma experiência do Assistant para acessar o DynamoMCP.
### O que o DynamoMCP pode fazer

O DynamoMCP fornece ao Assistant um conjunto de **ferramentas**. Cada ferramenta executa uma tarefa no Dynamo, como ler a tela, colocar um nó ou executar o gráfico. Você nunca chama essas ferramentas diretamente. Basta descrever o que deseja em linguagem simples, e o Assistant selecionará as ferramentas necessárias.

Ainda assim, conhecer essas ferramentas é útil. Isso permite entender o que o Assistant é capaz de fazer e ajuda você a formular solicitações que ele possa executar.

{% hint style="info" %}
Nomes de ferramentas que começam com `get_` somente leem informações da sessão. Todas as outras ferramentas podem alterar o gráfico.
{% endhint %}

#### Ler o gráfico atual

Estas ferramentas permitem que o Assistant veja no que você está trabalhando antes de responder ou fazer uma alteração.

| Ferramenta | O que faz? |
| --- | --- |
| `get_workspace_info` | Lista o que está na tela: nós, conexões, grupos, notas e detalhes do espaço de trabalho. |
| `get_selection` | Informa quais nós, conexões e grupos estão selecionados. |
| `get_node_connections` | Mostra a quais elementos cada nó está conectado, porta por porta. |
| `get_node_property` | Informa as configurações dos nós, como visualização, congelamento, visualização fixada e legendas. |
| `get_node_lacing` | Informa a configuração de amarra de um nó. |
| `get_installation_info` | Informa a versão do Dynamo, o hospedeiro em execução e um link para as notas de versão. |

*Exemplo de prompt: “O que está na minha tela no momento?”*

#### Encontrar o nó correto

O Dynamo tem milhares de nós, incluindo os disponibilizados pelos pacotes instalados Estas ferramentas ajudam o Assistant a escolher o nó correto, em vez de fazer suposições.

| Ferramenta | O que faz? |
| --- | --- |
| `get_nodes_info` | Pesquisa a biblioteca de nós, incluindo os nós de pacotes, e retorna o nó exato a ser colocado. |
| `get_node_help` | Retorna a ajuda escrita de um nó e o caminho para seu gráfico de exemplo, quando disponível. |
| `get_type_schema` | Retorna o formato de dados esperado para um tipo do Dynamo, como Ponto, Número ou Cor. |

*Exemplo de prompt: “Qual nó devo usar para obter a área de uma superfície?”*

#### Criar e editar o gráfico

Estas ferramentas fazem o trabalho na tela.

| Ferramenta | O que faz? |
| --- | --- |
| `create_nodes` | Coloca um ou mais nós na tela, com valores iniciais opcionais. |
| `set_node_value` | Define o valor dos nós de entrada, como Número, Sequência, Bloco de código e menus suspensos. |
| `connect_nodes` | Conecta uma porta de saída a uma porta de entrada. |
| `disconnect_nodes` | Remove conexões entre dois nós. |
| `delete_node` | Remove um nó da tela. |
| `rename_node` | Renomeia os nós de entrada e de inspeção para que sua finalidade fique clara. Os nós de função mantêm seus nomes originais. |
| `select_nodes` | Seleciona os nós na tela para indicar visualmente quais o Assistente está mencionando. |
| `set_node_property` | Ativa ou desativa as configurações dos nós: visualização, congelamento, visualização fixada, legendas e entrada ou saída. |
| `set_node_lacing` | Define o modo de amarração como Automático, Mais curto, Mais longo ou Produto cartesiano. |
| `set_node_port_list_at_level` | Define o nível da lista em uma porta de entrada. |

*Exemplo de prompt: “Adicione um controle deslizante numérico e conecte-o à entrada de raio de meu círculo.”*

#### Organizar e documentar o gráfico

Um gráfico funcional nem sempre é legível. Estas ferramentas lidam com layout, agrupamento, cores e notas.

| Ferramenta | O que faz? |
| --- | --- |
| `create_group` | Coloca um grupo em torno de um conjunto de nós. |
| `create_groups` | Cria vários grupos em uma etapa. |
| `ungroup_by_guid` | Remove um grupo, mantendo os nós na tela. |
| `ungroup_by_title` | Remove grupos pelo título, mantendo os nós na tela. |
| `get_available_group_styles` | Lista os estilos de grupo disponíveis, incluindo os estilos personalizados definidos nas preferências. |
| `assign_group_styles` | Aplica estilos de grupo, como Entradas, Ações, Saídas e Revisão. |
| `get_notes` | Lista as notas no gráfico. |
| `create_notes` | Adiciona notas para explicar partes do gráfico. |
| `update_notes` | Edita ou move o texto de uma nota. |
| `delete_notes` | Remove notas. |
| `set_note_state` | Fixa uma nota a um nó para que ambos se movam juntos ou desfaz essa associação. |
| `auto_layout_workspace` | Reorganiza os nós usando o layout automático integrado do Dynamo. |
| `set_workspace_description` | Grava uma descrição do gráfico nas propriedades do arquivo. |

*Exemplo de prompt: “Agrupe e aplique cores ao gráfico por entradas, ações e saídas e, em seguida, adicione notas curtas.”*

#### Executar o gráfico e verificar os resultados

| Ferramenta | O que faz? |
| --- | --- |
| `run_workspace` | Executa o gráfico atual. No modo automático, o gráfico já é executado novamente em cada alteração, portanto, essa etapa é ignorada. |
| `get_workspace_warnings` | Lista os nós com avisos ou erros, as mensagens correspondentes e as próximas etapas sugeridas. O gráfico precisa ser executado primeiro. |
| `get_node_output_values_shapes` | Resume os valores de saída de um nó ou de todos os nós do gráfico. |
| `get_node_output_value` | Lê a saída completa de um único nó, uma página por vez, para resultados que são muito grandes para serem resumidos. |

*Exemplo de prompt: “Execute o gráfico e me diga por que o nó List.GetItemAtIndex está exibindo um aviso.”

#### Trabalhar com arquivos de gráficos

O Assistant também pode trabalhar com gráficos salvos no disco, não apenas com o gráfico aberto.

| Ferramenta | O que faz? |
| --- | --- |
| `new_workspace` | Abre um novo gráfico vazio. Se o gráfico atual tiver alterações não salvas, você deverá salvá-las ou descartá-las primeiro. |
| `save_workspace` | Salva o gráfico atual. Se nenhuma localização for especificada, ele salva na pasta do Dynamo em Documentos. |
| `edit_graph_from_path` | Abre um gráfico salvo para edição, sem executá-lo. |
| `insert_graph_from_path` | Mescla um gráfico salvo na tela atual. É assim que os gráficos de exemplo de nós são inseridos. |
| `run_graph_from_path` | Executa um gráfico salvo, com valores de entrada opcionais fornecidos por você. |
| `run_graph_from_json` | Executa um gráfico fornecido como JSON do Dynamo, sem nenhum arquivo no disco. |
| `get_graph_info_from_path` | Lê as entradas, as saídas e os metadados de um gráfico salvo sem abri-lo. |
| `get_graph_info_from_json` | Lê os mesmos detalhes de um gráfico fornecido como JSON do Dynamo. |
| `get_graphs_info_from_path` | Examina uma pasta de gráficos e informa o que cada um contém. |
| `get_available_graphs_folders` | Lista as pastas conhecidas pelo Dynamo que podem conter gráficos. |

*Exemplo de prompt: “Examine minha pasta de gráficos do Dynamo e me diga quais gráficos colocam paredes”.*

{% hint style="warning" %}
O Assistant pode alterar o gráfico. Salve o trabalho antes de solicitar uma edição grande e revise o resultado antes de salvá-lo em um arquivo existente.
{% endhint %}
