# Atualizar os pacotes e as bibliotecas do Dynamo para Dynamo 3.x e NET8

### Introdução <a href="#introduction" id="introduction"></a>

Esta seção contém informações sobre problemas que podem ocorrer durante a migração dos gráficos, dos pacotes e das bibliotecas para o Dynamo 3.x.

O Dynamo 3.0 é uma versão principal e algumas APIs foram alteradas ou removidas. A maior alteração que provavelmente afetará você como desenvolvedor ou usuário do Dynamo 3.x é a mudança para .NET8.

Dotnet/.NET é o tempo de execução que alimenta a linguagem C# na qual o Dynamo está escrito. Atualizamos para uma versão moderna deste tempo de execução junto com o resto do ecossistema da Autodesk.

Você pode ler mais em [nossa postagem de blog](https://dynamobim.org/dynamo-on-net-8/).
***

### Compatibilidade dos pacotes <a href="#package-compatibility" id="package-compatibility"></a>

#### Usar pacotes do Dynamo 2.x no Dynamo 3.x 
Como o Dynamo 3.x agora é executado no tempo de execução do .NET8, não é garantido que os pacotes que foram criados para o Dynamo 2.x (*usando o .NET48*) funcionem no Dynamo 3.x. Quando você tentar fazer download de um pacote no Dynamo 3.x que foi publicado de uma versão do Dynamo inferior a 3.0, receberá um aviso de que o pacote é de uma versão anterior do Dynamo. 

**Isso não significa que o pacote não funcionará** É simplesmente um aviso de que podem ocorrer problemas de compatibilidade e, em geral, é uma boa ideia verificar se há uma versão mais recente que tenha sido criada especificamente para o Dynamo 3.x.

Você também pode observar esse tipo de aviso nos arquivos de registro do Dynamo no momento da carga do pacote. Se tudo estiver funcionando corretamente, será possível ignorá-lo.

#### Usar pacotes do Dynamo 3.x no Dynamo 2.x 

É muito improvável que um pacote criado para o Dynamo 3.x (*usando .Net8*) funcione no Dynamo 2.x. Você também verá um aviso ao fazer o download de pacotes criados para versões mais recentes do Dynamo enquanto usa uma versão mais antiga.


