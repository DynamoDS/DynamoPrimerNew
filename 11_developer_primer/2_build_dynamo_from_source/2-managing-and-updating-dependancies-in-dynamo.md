# Gerenciar e atualizar dependências no Dynamo

### Quando este wiki se aplica

Ao trabalhar em um novo recurso, ou simplesmente atualizar uma dependência existente, você deve avaliar o seguinte antes de trazer uma nova dependência para o repositório do Dynamo.

### Considerações

1. Qual é a licença da dependência nova ou atualizada – apenas algumas licenças de código aberto são aprovadas sem consultar o departamento jurídico da ADSK.
   * Depois de resolver a licença, certifique-se de que a dependência e a versão estejam gravadas no wiki interno.
   * Se a licença for `LGPL`, `GPL` ou `Apache`, o arquivo de licença deverá ser copiado para a subpasta _“Licenças de código aberto”_ da compilação do Dynamo.
   * Se a licença for `LGPL`, o código-fonte completo de todos os componentes de terceiros, juntamente com cópias de texto de suas licenças de código aberto apropriadas, deverá ser carregado para [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. Se estiver atualizando, o tipo de licença mudou em relação à versão anterior?
3. A dependência é multiplataforma?
   * Ela tem componentes nativos (como `CEFSharp` ou `ImageMagick`)? Isso dificultará a implantação multiplataforma
   * Ela só tem referências do Windows? Se for o caso, não deverá ser como dependência do DynamoCore ou de outras partes multiplataforma do Dynamo (a camada do modelo).
4. A dependência foi corretamente incluída na pasta bin na compilação com todas as dependências necessárias?
   * Se estiver atualizando, alguns arquivos foram removidos como consequência da atualização? Esta versão do Dynamo destina-se a uma versão pontual de produtos hospedeiros? Se for o caso, será necessário manter os binários antigos até um ano de lançamento global para ser compatível com os instaladores de patches. Veja [aqui](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. A dependência ou sua árvore de dependências está em conflito com outras dependências existentes no Dynamo?
6. A dependência ou sua árvore de dependências está em conflito com dependências existentes em produtos que integram o Dynamo no processo (Revit, Civil etc.) – **Isso é importante, pois esses problemas só podem ser descobertos no momento da integração, a menos que o trabalho seja feito antecipadamente.**
