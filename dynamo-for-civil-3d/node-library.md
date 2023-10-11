# Biblioteca de nós

Mencionamos anteriormente que OS **nós** são os principais blocos de construção de um gráfico do Dynamo e eles são organizados em grupos lógicos na **biblioteca**. No Dynamo for Civil 3D, há duas categorias (ou **prateleiras**) na biblioteca que contêm nós dedicados para trabalhar com objetos do AutoCAD e do Civil 3D, como alinhamentos, perfis, corredores, referências de bloco etc. O restante da biblioteca contém nós que são mais genéricos por natureza e são consistentes em todas as “versões” do Dynamo (por exemplo, Dynamo for Revit, Dynamo Sandbox etc.).

{% hint style="info" %}
 Confira a seção [2-library.md](../3\_user\_interface/2-library.md "mention") para obter mais informações sobre como os nós da biblioteca principal do Dynamo são organizados. 
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Biblioteca de nós do Dynamo for Civil 3D</p></figcaption></figure>

> 1. Nós específicos para trabalhar com objetos do AutoCAD e do Civil 3D
> 2. Nós para uso geral
> 3. Nós de **pacotes** de terceiros que você pode instalar separadamente

{% hint style="warning" %}
 Se você usar os nós encontrados nas prateleiras do AutoCAD e do Civil 3D, o gráfico do Dynamo funcionará somente no Dynamo for Civil 3D. Se um gráfico do Dynamo for Civil 3D for aberto em outro lugar (no Dynamo for Revit, por exemplo), esses nós serão sinalizados com um aviso e não serão executados. 
{% endhint %}

{% hint style="info" %}
 **Por que há duas prateleiras separadas para o AutoCAD e o Civil 3D?**

Essa organização distingue os nós de objetos nativos do AutoCAD (linhas, polilinhas, referências de bloco etc.) dos nós de objetos do Civil 3D (alinhamentos, corredores, superfícies etc.). E, do ponto de vista técnico, o AutoCAD e o Civil 3D são dois itens separados – o AutoCAD é o aplicativo base e o Civil 3D é construído com base nele. 
{% endhint %}

## Hierarquia de nós

Para trabalhar com os nós do AutoCAD e do Civil 3D, é importante ter uma compreensão sólida da hierarquia de objetos em cada prateleira. Você se lembra da taxonomia da biologia? Reino, Filo, Classe, Ordem, Família, Gênero, Espécie? Os objetos do AutoCAD e do Civil 3D são categorizados de forma semelhante. Vamos analisar alguns exemplos para explicar.

### Objetos do Civil

Vamos usar um alinhamento como exemplo.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Digamos que seu objetivo seja alterar o nome do alinhamento. A partir daqui, o próximo nó que você adicionaria é um nó **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

No início, isso pode não parecer muito intuitivo. O que é um **CivilObject** e por que a biblioteca não tem um nó **Alignment.SetName**? A resposta está relacionada à _reutilização_ e à _simplicidade._ Se você parar para pensar, o processo de alterar o nome de um objeto do Civil 3D será o mesmo quer o objeto seja um alinhamento, corredor, perfil ou outra coisa. Assim, em vez de ter nós repetitivos que essencialmente fazem a mesma coisa (por exemplo, **Alignment.SetName, Corridor.SetName, Profile.SetName** etc.), faria sentido combinar essa funcionalidade em um único nó. É exatamente isso que **CivilObject.SetName** faz.

Outra maneira de pensar sobre isso é com base em _relacionamentos_. Um alinhamento e um corredor são tipos de **Objetos do Civil**, assim como uma maçã e uma pera são tipos de frutas. Os nós de Objeto do Civil se aplicam a qualquer tipo de Objeto do Civil, da mesma forma que você pode desejar usar um único descascador para remover as cascas de uma maçã e de uma pera. Sua cozinha ficaria bem bagunçada se você tivesse um descascador separado para cada tipo de fruta. Nesse sentido, a biblioteca de nós do Dynamo é como sua cozinha.

### Objetos

Agora, vamos aprofundar essa ideia. Digamos que você deseje alterar a camada do alinhamento. Você usaria o nó **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Por que não há um nó chamado **CivilObject.SetLayer**? Os mesmos princípios de reutilização e simplicidade que discutimos anteriormente aplicam-se aqui. A propriedade _layer_ é algo que é comum a qualquer objeto no AutoCAD que possa ser desenhado ou inserido, como linha, polilinha, texto, referência de bloco etc. Os objetos do Civil 3D, como alinhamentos e corredores, estão na mesma categoria, e, portanto, qualquer nó que se aplica a um **Objeto** também pode ser usado com qualquer **Objeto do Civil**.

