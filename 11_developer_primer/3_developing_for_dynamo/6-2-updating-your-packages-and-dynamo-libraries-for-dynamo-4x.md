# Atualizar os pacotes e as bibliotecas do Dynamo para o Dynamo 4.x

### Introdução <a href="#introduction" id="introduction"></a>

Esta seção contém informações sobre problemas que podem ocorrer durante a migração dos gráficos, dos pacotes e das bibliotecas para o Dynamo 4.x. O Dynamo 4.0 apresenta:
* Melhorias significativas no desempenho
* Atualizações de estabilidade e correção de erros
* Modernização da base de código
* Remoção de APIs anteriormente marcadas como obsoletas na versão 1.x
* Uma grande atualização do tempo de execução do .NET 8 para o .NET 10
* O PythonNet3 agora é o mecanismo Python padrão para todos os novos nós do Python

O esforço de migração do .NET 10 garante que o Dynamo permaneça alinhado com o roteiro de tecnologia da Microsoft, bem antes do fim do suporte do .NET 8 em novembro de 2026.

Ao iniciar o Dynamo 4.0, será solicitado que você atualize para o .NET 10, caso ainda não tenha feito isso. Os autores de pacotes precisam atualizar seus projetos para o .NET 10 para garantir compatibilidade total.

Todos os novos nós Python criados no Dynamo 4.0 e superior começam com o PythonNet3. Não se preocupe com a compatibilidade com versões anteriores: para aqueles que trabalham em ambientes com várias versões (por exemplo, Revit ou Civil 3D 2025/2026), instale o pacote PythonNet3 Engine no Dynamo 3.3 a 3.6 para manter a compatibilidade. Para obter mais informações, clique [aqui](https://dynamobim.org/dynamo-core-4-0-release/).

A API e os nós que foram marcados como obsoletos na versão 1.x foram removidos no Dynamo 4.0. Você pode consultar a [lista completa de alterações aqui](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Compatibilidade dos pacotes <a href="#package-compatibility" id="package-compatibility"></a>

#### Usar pacotes do Dynamo 2.x e 3.x no Dynamo 4.x 
Como o Dynamo 4.x agora é executado no tempo de execução do .NET 10, não é garantido que os pacotes que foram criados para o Dynamo 2.x (*usando o .NET48*) e o Dynamo 3.x(*usando o .NET 8*) funcionem no Dynamo 4.x. Quando você tentar fazer download de um pacote no Dynamo 4.x que foi publicado de uma versão do Dynamo inferior a 4.0, receberá um aviso de que o pacote é de uma versão anterior do Dynamo.

**Isso não significa que o pacote não funcionará**. É simplesmente um aviso de que podem ocorrer problemas de compatibilidade e, em geral, é uma boa ideia verificar se há uma versão mais recente que tenha sido criada especificamente para o Dynamo 4.x.

Você também pode observar esse tipo de aviso nos arquivos de registro do Dynamo no momento do carregamento do pacote. Se tudo estiver funcionando corretamente, será possível ignorá-lo.

#### Usar pacotes do Dynamo 4.x no Dynamo 2.x 

É muito improvável que um pacote criado para o Dynamo 4.x (*usando o .Net 10*) funcione no Dynamo 2.x. Você também verá o aviso abaixo ao tentar instalar pacotes criados para o Dynamo 4.x no Dynamo 2.x.

![Aviso de compatibilidade de pacotes](images/6-2-packages-new-version-compatibility-warning.png)


#### Usar pacotes do Dynamo 4.x no Dynamo 3.x 

O pacote criado para o Dynamo 4.x (*usando o .NET 10*) pode funcionar no Dynamo 3.x, desde que todas as APIs usadas no pacote existam no .NET 8. Mas não há garantia de que funcionará. Você também verá o aviso abaixo ao tentar instalar pacotes criados para o Dynamo 4.x no Dynamo 3.x.

![Aviso de compatibilidade de pacotes](images/6-2-packages-compatibility-warning.png)

#### Práticas recomendadas para os autores de pacotes 
A prática recomendada é realizar o direcionamento múltiplo do projeto para o .NET 8 e o .NET 10 modificando o .csproj.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Isso garante:
* Suporte para versões do Dynamo hospedadas no Revit ainda no .NET8
* Compatibilidade com o Dynamo 4.x independente no .NET 10