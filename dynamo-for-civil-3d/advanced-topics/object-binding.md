# Vinculação de objetos

O Dynamo for Civil 3D contém um mecanismo muito poderoso para “lembrar” os objetos que são criados por cada nó. Esse mecanismo é chamado de **Vinculação de objetos** e permite que um gráfico do Dynamo produza resultados consistentes sempre que for executado no mesmo documento. Embora isso seja altamente desejável em muitas situações, há outras situações em que você pode desejar ter mais controle sobre o comportamento do Dynamo. Esta seção ajudará a entender como funciona a vinculação de objetos e como você pode tirar proveito dela.

## Exemplo

Considere este gráfico que cria um círculo no espaço do modelo na camada atual.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Um gráfico simples para criar um círculo</p></figcaption></figure>

Observe o que acontece quando o raio é alterado.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Modificação da entrada de raio no Dynamo</p></figcaption></figure>

É a vinculação de objetos em ação. O comportamento padrão do Dynamo é _modificar_ o raio do círculo, em vez de criar um novo círculo sempre que a entrada do raio é alterada. Isso ocorre porque o nó **Object.ByGeometry** “lembra” que criou esse círculo _específico_ sempre que o gráfico é executado. Além disso, o Dynamo armazenará essas informações de modo que, na próxima vez que você abrir o documento do Civil 3D e executar o gráfico, ele tenha exatamente o mesmo comportamento.

## Outro exemplo

Vamos examinar um exemplo no qual você pode desejar alterar o comportamento da vinculação de objetos padrão do Dynamo. Digamos que você deseje criar um gráfico que posicione o texto no meio de um círculo. No entanto, sua intenção com esse gráfico é que ele possa ser executado repetidamente e posicionar novo texto sempre que qualquer círculo for selecionado. Veja a seguir a aparência do gráfico.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Um gráfico simples que posiciona texto no centro de um círculo selecionado</p></figcaption></figure>

No entanto, isso é o que acontece quando um círculo diferente é selecionado.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Comportamento padrão do Dynamo ao selecionar um novo círculo</p></figcaption></figure>

Parece que o texto é excluído e recriado a cada execução do gráfico. Na realidade, a posição do texto está sendo _modificada_, dependendo de qual círculo está selecionado. Portanto, é o mesmo texto, só que em um lugar diferente. Para criar um novo texto a cada vez, é necessário modificar as configurações de vinculação de objetos do Dynamo para que nenhum dado de vinculação seja mantido (consulte [\#binding-settings](object-binding.md#binding-settings "mention") abaixo).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Configurações de vinculação de objetos</p></figcaption></figure>

Depois de fazer essa alteração, temos o comportamento que procuramos.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Comportamento com a vinculação de objetos desativada</p></figcaption></figure>

## Configurações de vinculação

O Dynamo for Civil 3D permite modificar o comportamento padrão da vinculação de objetos por meio das configurações **Armazenamento de dados de vinculação** no menu do **Dynamo**.

{% hint style="info" %} Observe que as opções de Armazenamento de dados de vinculação estão disponíveis no **Civil 3D 2022.1** e versões posteriores. {% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Todas as opções estão ativadas por padrão. Veja a seguir um resumo do que cada opção faz.

### Opção 1: Nenhum dado de vinculação mantido

Quando essa opção está ativada, o Dynamo “esquecerá” os objetos que criou na última vez que o gráfico foi executado. Assim, o gráfico pode ser executado em qualquer desenho em qualquer situação e criará novos objetos a cada vez.

{% hint style="info" %} **Quando usar**

Use essa opção quando desejar que o Dynamo “esqueça” tudo sobre o que fez em execuções anteriores e crie novos objetos todas as vezes. {% endhint %}

### Opção 2: Armazenar no desenho para o Dynamo

Essa opção significa que os metadados da vinculação de objetos serão serializados no gráfico (arquivo .dyn) quando ele for salvo. Se você fechar/reabrir o gráfico e executá-lo no **mesmo desenho**, tudo deverá funcionar da mesma forma que você deixou. Se o gráfico for executado em um **desenho diferente**, os dados de vinculação serão removidos do gráfico e novos objetos serão criados. Isso significa que, se você abrir o desenho original e executar o gráfico novamente, serão criados novos objetos além dos antigos.

{% hint style="info" %} **Quando usar**

Use essa opção quando desejar que o Dynamo “lembre-se” dos objetos que criou na última vez que foi executado em um **desenho específico**. {% endhint %}

{% hint style="warning" %} Essa opção é mais adequada para situações em que é possível manter uma relação 1:1 entre um **desenho específico** e um gráfico do Dynamo. As opções 1 e 3 são mais adequadas para gráficos projetados para execução em vários desenhos. {% endhint %}

### Opção 3: Armazenar no desenho para o Dynamo

Essa opção é semelhante à Opção 2, exceto que os dados de vinculação de objetos são serializados no desenho em vez de serem no gráfico (arquivo .dyn). Se você fechar/reabrir o gráfico e executá-lo no **mesmo desenho**, tudo deverá funcionar da mesma forma que você deixou. Se o gráfico for executado em um **desenho diferente**, os dados de vinculação ainda serão preservados no desenho original, já que ele é salvo no desenho e não no gráfico.

{% hint style="info" %} **Quando usar**

Use essa opção quando desejar usar o mesmo gráfico em **vários desenhos** e que o Dynamo “lembre” do que fez em cada um deles. {% endhint %}

### Opção 4: Armazenar no desenho para o Reprodutor do Dynamo

A primeira coisa a observar com essa opção é que ela não tem efeito sobre como o gráfico interage com o desenho ao executar o gráfico através da interface principal do Dynamo. Essa opção _somente_ se aplica quando o gráfico é executado usando o Reprodutor do Dynamo.

{% hint style="info" %} Se o Reprodutor do Dynamo for algo novo para você, veja a seção [dynamo-player.md](../dynamo-player.md "mention"). {% endhint %}

Se o gráfico for executado usando a interface principal do Dynamo e, em seguida, fechar e executar o mesmo gráfico usando o Reprodutor do Dynamo, ele criará novos objetos além dos que criou antes. No entanto, quando o Reprodutor do Dynamo executar o gráfico uma vez, ele serializará os dados da vinculação de objetos no desenho. Portanto, se o gráfico for executado várias vezes através do Reprodutor do Dynamo, ele atualizará os objetos em vez de criar novos. Se o gráfico for executado através do Reprodutor do Dynamo em um **desenho diferente**, os dados de vinculação ainda serão preservados no desenho original, já que ele será salvo no desenho e não no gráfico.

{% hint style="info" %} **Quando usar**

Use essa opção quando desejar executar um gráfico usando o Reprodutor do Dynamo em vários desenhos e que ele “lembre” o que fez em cada um deles. {% endhint %}
