# Python e Civil 3D

Embora o Dynamo seja extremamente poderoso como uma ferramenta de [programação visual](../../a\_appendix/a-1\_visual-programming-and-dynamo.md), também é possível ir além dos nós e fios e escrever código em forma textual. Há duas maneiras de fazer isso:

1. Escrever **DesignScript** usando um bloco de código
2. Escrever **Python** usando um nó Python

Esta seção se concentrará em como aproveitar o Python no ambiente do Civil 3D para tirar proveito das APIs .NET do AutoCAD e do Civil 3D.

{% hint style="info" %}
 Dê uma olhada na seção [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") para obter informações mais gerais sobre como usar o Python no Dynamo. 
{% endhint %}

## Documentação das APIs

O AutoCAD e o Civil 3D têm várias APIs disponíveis que permitem que desenvolvedores como você estendam o produto principal com funcionalidade personalizada. No contexto do Dynamo, são as **APIs .NET gerenciadas** que são relevantes. Os links a seguir são essenciais para entender a estrutura das APIs e como elas funcionam.

[Guia do Desenvolvedor da API .NET do AutoCAD](https://help.autodesk.com/view/OARX/2024/PTB/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Guia de Referência da API .NET do AutoCAD](https://help.autodesk.com/view/OARX/2024/PTB/?guid=OARX-ManagedRefGuide-What_s_New)

[Guia do Desenvolvedor de API .NET do Civil 3D](https://help.autodesk.com/view/CIV3D/2024/PTB/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Guia de Referência da API .NET do Civil 3D](https://help.autodesk.com/view/CIV3D/2024/PTB/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %}
 Ao longo desta seção, pode haver alguns conceitos com os quais você não está familiarizado, como bancos de dados, transações, métodos, propriedades etc. Muitos desses conceitos são fundamentais para trabalhar com as APIs .NET e não são específicos do Dynamo ou do Python. Está além do escopo desta seção do Manual discutir esses itens em detalhes; portanto, recomendamos consultar os links acima com frequência para obter mais informações. 
{% endhint %}

## Modelo de código

Quando você editar um novo nó Python pela primeira vez, ele será preenchido previamente com o código do modelo para começar. Veja a seguir um detalhamento do modelo com explicações sobre cada bloco.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Modelo Python padrão no Civil 3D</p></figcaption></figure>

> 1. Importa os módulos `sys` e `clr`, que são necessários para que o interpretador Python funcione corretamente. Em particular, o módulo `clr` permite que os namespaces .NET sejam tratados essencialmente como pacotes Python.
> 2. Carrega as montagens padrão (ou seja, DLLs) para trabalhar com as APIs .NET gerenciadas para o AutoCAD e o Civil 3D.
> 3. Adiciona referências aos namespaces padrão do AutoCAD e do Civil 3D. Isso é equivalente às diretivas `using` ou `Imports` em C# ou VB.NET (respectivamente).
> 4. As portas de entrada do nó são acessíveis usando uma lista predefinida chamada `IN`. É possível acessar os dados em uma porta específica usando seu número de índice, por exemplo, `dataInFirstPort = IN[0]`.
> 5. Obtém o documento e o editor ativos.
> 6. Bloqueia o documento e inicia uma transação do banco de dados.
> 7. Aqui você deve colocar a maior parte da lógica do script.
> 8. Retire o comentário dessa linha para confirmar a transação após seu trabalho principal ter sido concluído.
> 9. Se desejar gerar dados do nó, atribua-os à variável `OUT` no final do script.

{% hint style="info" %}
 **Deseja personalizar?**\
 É possível modificar o modelo Python padrão editando o arquivo `PythonTemplate.py` localizado em `C:\ProgramData\Autodesk\C3D <versão>\Dynamo`. 
{% endhint %}

## Exemplo

Vamos analisar um exemplo para demonstrar alguns dos conceitos essenciais de escrever scripts Python no Dynamo for Civil 3D.

### Objetivo

> :dart: Obter a geometria de limite de todas as áreas de contribuição em um desenho.

### Conjunto de dados

Veja a seguir arquivos de exemplos que você pode consultar para este exercício.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Visão geral da solução

Apresentamos a seguir uma visão geral da lógica no gráfico.

> 1. Revisar a documentação da API do Civil 3D
> 2. Selecionar todas as áreas de contribuição no documento por nome de camada
> 3. “Expandir” os objetos do Dynamo para acessar os membros internos da API do Civil 3D
> 4. Criar pontos do Dynamo com base em pontos do AutoCAD
> 5. Criar PolyCurves com base em pontos

Vamos começar

### Revisar a documentação da API

Antes de começarmos a criar nosso gráfico e escrever código, é uma boa ideia dar uma olhada na documentação da API do Civil 3D e ter uma ideia do que a API disponibiliza para nós. Neste caso, há uma [propriedade na classe Área de contribuição](https://help.autodesk.com/view/CIV3D/2024/PTB/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c) que retornará os pontos de limite da área de contribuição. Observe que essa propriedade retorna um objeto `Point3dCollection`, que o Dynamo não saberá como tratar. Em outras palavras, não poderemos criar uma PolyCurve com base em um objeto `Point3dCollection`; portanto, acabaremos precisando converter tudo em pontos do Dynamo. Haverá mais informações sobre isso mais tarde.

### Obter todas as áreas de contribuição

Agora podemos começar a criar nossa lógica do gráfico. A primeira coisa a fazer é obter uma lista de todas as áreas de contribuição no documento. Há nós disponíveis para isso; portanto, não precisamos incluí-los no script Python. O uso de nós oferece uma melhor visibilidade para outra pessoa que possa ler o gráfico (em vez de encher o script Python de código) e também mantém o script Python focado em uma coisa: retornar os pontos de limite das áreas de contribuição.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Obtenção de todas as áreas de contribuição no documento por camada</p></figcaption></figure>

Observe aqui que a saída do nó **Todos os objetos na camada** é uma lista de CivilObjects. Isso ocorre porque o Dynamo for Civil 3D não tem nós atualmente para trabalhar com áreas de contribuição, o que é a razão pela qual precisamos acessar a API por meio do Python.

### Expandir objetos

Antes de avançarmos, temos de abordar brevemente um conceito importante. Na seção [node-library.md](../node-library.md "mention"), discutimos como Objetos e CivilObjects são relacionados. Para adicionar um pouco mais de detalhes a isso, um **Objeto do Dynamo** é um wrapper em torno de uma **Entidade do AutoCAD**. De forma similar, um **CivilObject do Dynamo** é um wrapper em torno de uma **Entidade do Civil 3D**. É possível “expandir” um objeto acessando suas propriedades `InternalDBObject` ou `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Tipo do Dynamo</th><th width="373">Pacotes</th></tr></thead><tbody><tr><td><strong>Objeto</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entidade</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entidade</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %}
 Como regra geral, é mais seguro obter a ID de objeto usando a propriedade `InternalObjectId` e, em seguida, acessar o objeto empacotado em uma transação. Isso ocorre porque a propriedade `InternalDBObject` retornará um DBObject do AutoCAD que não está em um estado gravável. 
{% endhint %}

### Script Python

Aqui está o script Python completo que faz o trabalho de acessar os objetos de área de contribuição internos que estão obtendo seus pontos de limite. As linhas realçadas representam as que são modificadas/adicionadas do código do modelo padrão.

{% hint style="info" %}
 Clique no texto sublinhado no script para obter uma explicação sobre cada linha. 
{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Carregar as bibliotecas Standard e do DesignScript do Python
import sys
import clr

# Adicionar montagens para o AutoCAD e o Civil 3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Importar referências do AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Importar referências do Civil3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# As entradas para esse nó serão armazenadas como uma lista nas variáveis IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit(“A entrada é nula ou está vazia.”)</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">para acPnt em acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Confirmar antes de finalizar a transação
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Atribua a saída à variável OUT.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %}
 Como regra geral, é recomendável incluir a maior parte da lógica do script dentro de uma transação. Isso garante o acesso seguro aos objetos que o script está lendo/gravando. Em muitos casos, a omissão de uma transação pode causar um erro fatal. 
{% endhint %}

### Criar PolyCurves

Nesta fase, o script Python deve gerar uma lista de pontos do Dynamo que você pode ver na visualização do plano de fundo. A última etapa é simplesmente criar PolyCurves com base nos pontos. Observe que isso também pode ser feito diretamente no script Python, mas nós o colocamos intencionalmente fora do script em um nó para que ele fique mais visível. Veja a seguir a aparência final do gráfico.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Gráfico final</p></figcaption></figure>

### Resultado

E aqui está a geometria final do Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>PolyCurves do Dynamo resultantes para os limites da área de contribuição</p></figcaption></figure>

> :tada: Missão cumprida.

## IronPython versus CPython

Apenas uma rápida observação aqui antes de finalizarmos. Dependendo de qual versão do Civil 3D você está usando, o nó Python pode ser configurado de forma diferente. No **Civil 3D 2020 e 2021**, o Dynamo usava uma ferramenta chamada **IronPython** para mover dados entre objetos .NET e scripts Python. No entanto, no **Civil 3D 2022**, o Dynamo fez a transição para usar o interpretador Python nativo padrão (também conhecido como **CPython**), que usa o Python 3. Os benefícios dessa transição incluem o acesso a bibliotecas modernas e populares, além de novos recursos de plataforma, manutenção essencial e patches de segurança.

{% hint style="info" %}
 Você pode ler mais sobre essa transição e sobre como atualizar os scripts herdados no [Blog do Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Se você desejar continuar usando o IronPython, basta instalar o pacote **DynamoIronPython2.7** usando o Dynamo Package Manager. 
{% endhint %}

[^1]: Por padrão, a biblioteca de geometria do Dynamo não é adicionada ao ambiente Python. Nosso objetivo com este script é gerar uma lista de pontos do Dynamo para os limites da área de contribuição; portanto, precisamos adicionar essa linha para criar os pontos mais tarde.

[^2]: Essa linha obtém a classe específica que precisamos da biblioteca de geometria do Dynamo. Observe que especificamos `import Point as DynPoint` aqui em vez de `import *`, pois introduziria interferências de nomenclatura.

[^3]: Aqui, renomeamos a variável padrão `dataEnteringNode` como `objs` para que ela seja mais descritiva.

[^4]: Aqui especificamos exatamente qual porta de entrada contém os dados que desejamos em vez do padrão `IN`, que faz referência à lista inteira de todas as entradas.

[^5]: Isso cria uma lista vazia à qual adicionaremos os dados de saída mais tarde.

[^6]: Isso serve para proteção contra uma entrada vazia ou nula potencial em nosso script.

[^7]: Em vez de interromper o script, geraremos uma mensagem útil para explicar por que o script não pode continuar.

[^8]: O restante do script presume que temos uma lista de objetos com os quais trabalharemos (em vez de um único objeto). Mas, caso exista apenas um objeto (ou seja, um desenho com uma única área de contribuição), ainda queremos que o script possa ser executado.

[^9]: Se a entrada não for uma lista (ou seja, um único objeto), simplesmente criaremos uma nova lista com o objeto como único item.

[^10]: Inicie um contorno para cada objeto na lista.

[^11]: “Expanda” o objeto do Dynamo obtendo sua ID de objeto.

[^12]: Recupere o objeto “empacotado” do banco de dados do AutoCAD. Observe que o OpenMode está definido como `ForRead` aqui porque não estamos planejando fazer nenhuma edição nos objetos. Estamos simplificando a “consulta” de dados.

[^13]: É possível que a lista de entrada de objetos contenha uma mistura de áreas de contribuição e outros itens que não sejam áreas de contribuição. Precisamos verificar essa situação e lidar com ela adequadamente (ou seja, só continue esta iteração do contorno se o item for de fato uma área de contribuição).

[^14]: Se o script chegar até aqui, saberemos que o objeto é realmente uma área de contribuição. Vamos adicionar uma nova variável aqui apenas para manter a nomenclatura limpa e compreensível.

[^15]: Aqui é onde recuperamos os pontos de limite da área de contribuição usando a propriedade apropriada que pesquisamos anteriormente nos documentos da API. Como mencionado anteriormente, isso nos dará um objeto `Point3dCollection`, que é essencialmente uma lista de pontos do AutoCAD. Precisaremos “convertê-los” em pontos do Dynamo para que sejam úteis.

[^16]: Crie uma lista vazia para armazenar os pontos de limite para essa área de contribuição.

[^17]: Inicie um contorno para cada objeto `Point3d` na `Point3dCollection`.

[^18]: Crie um ponto do Dynamo usando as coordenadas do ponto do AutoCAD.

[^19]: Adicione o ponto à lista.

[^20]: Quando terminarmos de “converter” todos os pontos do AutoCAD em pontos do Dynamo, adicionaremos a lista resultante à nossa saída. Depois disso, o contorno externo será movido para o próximo objeto na lista de entrada.

[^21]: Retire o comentário dessa linha para confirmar a transação.

[^22]: E por último, mas não menos importante, geramos a lista de pontos do Dynamo.
