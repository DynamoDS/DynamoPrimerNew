# Creazione di un pacchetto da Visual Studio

Se si stanno sviluppando assiemi da pubblicare come pacchetto per Dynamo, il progetto può essere configurato per raggruppare tutti i componenti di progetto necessari e inserirli in una struttura di directory compatibile con il pacchetto. Ciò consentirà di testare rapidamente il progetto come pacchetto e di simulare l'esperienza dell'utente.

#### Come creare direttamente un pacchetto nella cartella corrispondente <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

In Visual Studio sono disponibili due metodi per la creazione di un pacchetto:

* Aggiungere eventi post-compilazione tramite la finestra di dialogo Project Settings che utilizzano script xcopy o Python per copiare i file necessari
* Utilizzare la destinazione della build "AfterBuild" nel file `.csproj` per creare le operazioni di copia di file e directory.

"AfterBuild" è il metodo preferito per questi tipi di operazioni (e quello descritto in questo manuale), poiché non si basa sulla copia di file che potrebbe non essere disponibile nel computer di compilazione.

#### Copia di file di pacchetto con il metodo AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Impostare la struttura delle directory nel repository in modo che i file di origine siano separati dai file di pacchetto. Utilizzando il case study CustomNodeModel, posizionare il progetto di Visual Studio e tutti i file associati in una nuova cartella `src`. Verranno memorizzati tutti i pacchetti generati dal progetto in questa cartella. La struttura delle cartelle dovrebbe ora essere simile alla seguente:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Spostamento dei file di progetto](images/fe-proj-directory.jpg)

> 1. Spostare i file di progetto nella nuova cartella `src`

Ora che i file di origine si trovano in una cartella separata, aggiungere una destinazione `AfterBuild` al file `CustomNodeModel.csproj` in Visual Studio. In questo modo, i file necessari dovrebbero essere copiati in una nuova cartella del pacchetto. Aprire il file `CustomNodeModel.csproj` in un editor di testo (è stato utilizzato [Atom](https://atom.io)) e posizionare la destinazione della build prima del tag `</Project>` di chiusura. Questa destinazione AfterBuild copierà tutti i file .dll, .pbd, .xml e .config in una nuova cartella bin e creerà una cartella dyf e cartelle extra.

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

![Posizionamento della destinazione AfterBuild](images/atom-afterbuild.jpg)

> Sarà necessario verificare che la destinazione sia stata aggiunta al file `CustomNodeModel.csproj` (non ad un altro file di progetto) e che per il progetto non siano state definite impostazioni post-compilazione esistenti.
>
> 1. Posizionare la destinazione AfterBuild prima del tag `</Project>` finale.

Nella sezione `<ItemGroup>`, vengono definite diverse variabili per rappresentare tipi di file specifici. Ad esempio, la variabile `Dll` rappresenta tutti i file nella directory di output con estensione `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

L'operazione `Copy` consiste nel copiare tutti i file `.dll` in una directory, in particolare nella cartella del pacchetto in cui si sta eseguendo la compilazione.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

I pacchetti di Dynamo in genere dispongono di una cartella `dyf` e `extra` per i nodi personalizzati di Dynamo e altri componenti di progetto, ad esempio le immagini. Per creare queste cartelle, è necessario utilizzare un'attività `MakeDir`. Questa operazione creerà una cartella se non esiste. È possibile aggiungere i file manualmente a questa cartella.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Se si crea il progetto, la cartella di progetto dovrebbe ora avere una cartella `packages` accanto alla cartella `src` creata in precedenza. All'interno della directory `packages` è presente una cartella contenente tutti gli elementi necessari per il pacchetto. È inoltre necessario copiare il file `pkg.json` nella cartella del pacchetto in modo che Dynamo sappia di caricare il pacchetto.

![Copia di file](images/fe-proj-directory-package.jpg)

> 1. La nuova cartella del pacchetto creata dalla destinazione AfterBuild
> 2. La cartella src esistente con il progetto
> 3. Le cartelle `dyf` e `extra` create dalla destinazione AfterBuild
> 4. Copiare manualmente il file `pkg.json`.

Ora è possibile pubblicare il pacchetto utilizzando il gestore di pacchetti di Dynamo o copiarlo direttamente nella directory del pacchetto di Dynamo: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
