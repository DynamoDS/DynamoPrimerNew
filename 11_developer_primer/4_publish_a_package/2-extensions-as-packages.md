# Extensões como pacotes

### Extensões como pacotes <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### Visão geral <a href="#overview" id="overview"></a>

As extensões do Dynamo podem ser implantadas no gerenciador de pacotes, como as bibliotecas de nós normais do Dynamo. Quando um pacote instalado contém uma extensão de vista, a extensão é carregada no tempo de execução quando o Dynamo é carregado. É possível verificar o console do Dynamo para verificar se a extensão foi carregada corretamente.

### Estrutura do pacote <a href="#package-structure" id="package-structure"></a>

A estrutura de um pacote de extensão é a mesma que um pacote normal contendo...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

Supondo que você já tenha compilado sua extensão, terá (no mínimo) uma montagem .NET e um arquivo de manifesto. A montagem deve conter uma classe que implementa `IViewExtension` ou `IExtension`. O arquivo de manifesto .XML informa ao Dynamo qual classe deve ser instanciada para iniciar a extensão. Para que o gerenciador de pacotes localize corretamente a extensão, o arquivo de manifesto deve corresponder com precisão à localização e à nomenclatura da montagem.

Coloque os arquivos de montagem na pasta `bin` e o arquivo de manifesto na pasta `extra`. Também é possível colocar todos os recursos adicionais nessa pasta.

Exemplo de arquivo .XML de manifesto:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### Carregar <a href="#uploading" id="uploading"></a>

Depois que você tiver uma pasta contendo os subdiretórios descritos acima, estará pronto para enviar (carregar) para o gerenciador de pacotes. Um ponto a ter em mente é que não é possível publicar pacotes do Dynamo Sandbox. Isso significa que é necessário estar usando o Dynamo Revit. Uma vez dentro do Dynamo Revit, navegue até Pacotes => Publicar novo pacote. Isso solicitará que o usuário faça login na Autodesk Account com a qual deseja associar o pacote.

Neste ponto, você deve estar na janela normal do pacote de publicação, onde será necessário inserir todos os campos obrigatórios relativos ao pacote/extensão. Há uma etapa adicional **muito importante** que requer que você assegure que nenhum dos arquivos de montagem esteja marcado como uma biblioteca de nós. Isso é feito clicando com o botão direito do mouse nos arquivos importados (a pasta de pacotes criada acima). Um menu de contexto será exibido, o que lhe dá a opção de marcar (ou desmarcar) essa opção. Todas as montagens de extensão devem estar desmarcadas.

![Publicar um pacote](images/ViewExtension_Search.png)

Antes de publicar de forma pública, você deve sempre publicar localmente para assegurar que tudo funcione como esperado. Uma vez que isso tenha sido verificado, você estará pronto para entrar em ação selecionando Publicar.

### Extrair <a href="#pulling" id="pulling"></a>

Para verificar se o pacote foi carregado com êxito, você deverá conseguir pesquisá-lo com base na nomeação e nas palavras-chave especificadas na etapa de publicação. Por fim, é importante observar que as mesmas extensões exigirão uma reinicialização do Dynamo antes de funcionar. Normalmente, essas extensões exigem parâmetros especificados quando o Dynamo é inicializado.

![Procurar pacotes](images/ViewExtension_Search.jpg)
