# Compilar um pacote no Visual Studio

Se você estiver desenvolvendo montagens a serem publicadas como um pacote do Dynamo, o projeto poderá ser configurado para agrupar todos os recursos necessários e colocá-los em uma estrutura de diretório compatível com o pacote. Isso permitirá que o projeto seja rapidamente testado como um pacote e simulará a experiência do usuário.

#### Como compilar diretamente na pasta de pacotes <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Há dois métodos para compilar um pacote no Visual Studio:

* Adicionar eventos pós-compilação através da caixa de diálogo Configurações do projeto que usam scripts xcopy ou Python para copiar os arquivos necessários
* Usar o destino de compilação “AfterBuild” no arquivo `.csproj` para criar tarefas de cópia de arquivos e diretórios

“AfterBuild” é o método preferido para esses tipos de operações (e o que é abordado neste guia), pois não depende da cópia de arquivos que podem não estar disponíveis no computador de compilação.

#### Copiar arquivos de pacote com o método AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Configure a estrutura de diretórios no repositório para que os arquivos de origem sejam separados dos arquivos de pacote. Trabalhando com o estudo de caso CustomNodeModel, coloque o projeto do Visual Studio e todos os arquivos associados em uma nova pasta `src`. Todos os pacotes gerados pelo projeto serão armazenados nessa pasta. A estrutura de pastas deve ter este aspecto:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Mover os arquivos de projeto](images/fe-proj-directory.jpg)

> 1. Mover os arquivos de projeto para a nova pasta `src`

Agora que os arquivos de origem estão em uma pasta separada, adicione um destino `AfterBuild` ao arquivo `CustomNodeModel.csproj` no Visual Studio. Isso deve copiar os arquivos necessários para uma nova pasta de pacote. Abra o arquivo `CustomNodeModel.csproj` em um editor de texto (usamos o [Atom](https://atom.io)) e coloque o destino da compilação antes do identificador de fechamento `</Project>`. Esse destino AfterBuild copiará todos os arquivos .dll, .pbd, .xml e .config para uma nova pasta bin e criará as pastas dyf e extras.

```
  <Target Name="AfterBuild">
    <ItemGroup>
      <Dlls Include="$(OutDir)*.dll" />
      <Pdbs Include="$(OutDir)*.pdb" />
      <Xmls Include="$(OutDir)*.xml" />
      <Configs Include="$(OutDir)*.config" />
    </ItemGroup>
    <Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Pdbs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Xmls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Configs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\dyf" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
  </Target>
```

![Colocar o destino AfterBuild](images/atom-afterbuild.jpg)

> Precisamos ter certeza de que o destino foi adicionado ao arquivo `CustomNodeModel.csproj` (não a outro arquivo de projeto) e que o projeto não tem nenhuma configuração Pós-compilação existente.
>
> 1. Colocar o destino AfterBuild antes do identificador `</Project>` final.

Na seção `<ItemGroup>`, são definidas um número de variáveis para representar tipos de arquivo específicos. Por exemplo, a variável `Dll` representa todos os arquivos no diretório de saída cuja extensão é `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

O objetivo da tarefa `Copy` é copiar todos os arquivos `.dll` para um diretório, especificamente a pasta de pacotes para a qual estamos compilando.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Os pacotes do Dynamo normalmente têm uma pasta `dyf` e `extra` para os nós personalizados do Dynamo e outros recursos, como imagens. Para criar essas pastas, é preciso usar uma tarefa `MakeDir`. Essa tarefa criará uma pasta se ela não existir. É possível adicionar arquivos manualmente a essa pasta.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Se você compilar o projeto, a pasta do projeto agora deverá ter uma pasta `packages` junto com a pasta `src` criada anteriormente. Dentro do diretório `packages`, há uma pasta que contém tudo o que é necessário para o pacote. Também precisamos copiar o arquivo `pkg.json` para a pasta do pacote para que o Dynamo saiba como carregar o pacote.

![Copiar arquivos](images/fe-proj-directory-package.jpg)

> 1. A nova pasta de pacotes que o destino AfterBuild criou
> 2. A pasta src existente com o projeto
> 3. As pastas `dyf` e `extra` criadas no destino AfterBuild
> 4. Copiar manualmente o arquivo `pkg.json`.

Agora você pode publicar o pacote usando o gerenciador de pacotes do Dynamo ou copiá-lo diretamente para o diretório de pacotes do Dynamo: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
