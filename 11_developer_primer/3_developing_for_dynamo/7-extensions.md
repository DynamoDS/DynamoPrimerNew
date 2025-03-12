# Estensioni

Le estensioni sono un potente strumento di sviluppo nell'ecosistema di Dynamo. Consentono agli sviluppatori di gestire funzionalità personalizzate basate sulle interazioni e sulla logica di Dynamo. Le estensioni possono essere suddivise in due categorie principali, ovvero estensioni ed estensioni delle viste. Come implica la denominazione, il framework dell'estensione della vista consente di estendere l'interfaccia utente di Dynamo registrando voci di menu personalizzate. Le estensioni standard funzionano in modo molto simile, meno l'interfaccia utente. Ad esempio, è possibile creare un'estensione che registra informazioni specifiche nella console di Dynamo. Questo scenario non richiede alcuna interfaccia utente personalizzata e pertanto potrebbe essere eseguito anche utilizzando un'estensione.

#### Case study dell'estensione <a href="#extension-case-study" id="extension-case-study"></a>

Seguendo l'esempio SampleViewExtension dal repository DynamoSamples su GitHub, illustreremo la procedura necessaria per creare una finestra non modale semplice che visualizzi i nodi attivi nel grafico in tempo reale. Per un'estensione della vista è necessario creare un'interfaccia utente per la finestra ed eseguire il binding dei valori ad un modello di vista.

![Finestra dell'estensione della vista](images/dyn-viewextension.jpg)

> 1. La finestra dell'estensione della vista è stata sviluppata seguendo l'esempio SampleViewExtension nel repository su GitHub.

Anche se costruiremo l'esempio da zero, è possibile anche scaricare e creare il repository DynamoSamples come riferimento.

Il repository DynamoSamples: [https://github.com/DynamoDS/DynamoSamples](https://github.com/DynamoDS/DynamoSamples)

> Questa simulazione farà riferimento in modo specifico al progetto denominato SampleViewExtension disponibile in `DynamoSamples/src/`.

#### Come implementare un'estensione della vista <a href="#how-to-implement-a-view-extension" id="how-to-implement-a-view-extension"></a>

Un'estensione della vista è costituita da tre parti essenziali:

* Un assieme contenente una classe che implementa `IViewExtension` e una classe che crea un modello di vista
* Un file `.xml` che indica a Dynamo dove deve cercare questo assieme in fase di esecuzione e il tipo di estensione
* Un file `.xaml` che esegue il binding dei dati alla visualizzazione grafica e determina l'aspetto della finestra

**1\. Creazione della struttura del progetto**

Iniziare creando un nuovo progetto `Class Library` denominato `SampleViewExtension`.

![Creare una nuova libreria di classi](images/vs-new-project-viewextension-1.jpg).

![Configurazione di un nuovo progetto](images/vs-new-project-viewextension-2.jpg)

> 1. Creare un nuovo progetto selezionando `File > New > Project`.
> 2. Selezionare `Class Library`.
> 3. Assegnare al progetto il nome `SampleViewExtension`.
> 4. Selezionare `Ok`.

In questo progetto, avremo bisogno di due classi. Una classe implementerà `IViewExtension` e un'altra che implementerà `NotificationObject.` `IViewExtension` conterrà tutte le informazioni su come l'estensione verrà distribuita, caricata, utilizzata come riferimento ed eliminata. `NotificationObject` fornirà notifiche per le modifiche in Dynamo e `IDisposable`. Quando si verifica una modifica, il conteggio verrà aggiornato di conseguenza.

![File di classe dell'estensione della vista](images/vs-viewextension-classes.jpg)

> 1. Un file di classe denominato `SampleViewExtension.cs` che implementerà `IViewExtension`
> 2. Un file di classe denominato `SampleWindowViewMode.cs` che implementerà `NotificationObject`

Per utilizzare `IViewExtension`, è necessario il pacchetto NuGet WpfUILibrary. L'installazione di questo pacchetto comporta l'installazione automatica dei pacchetti Core, Services e ZeroTouchLibrary.

![Pacchetti dell'estensione della vista](images/vs-viewextension-packages.jpg)

> 1. Selezionare WpfUILibrary.
> 2. Selezionare `Install` per installare tutti i pacchetti dipendenti.

**2\. Implementazione della classe IViewExtension**

Dalla classe `IViewExtension` determineremo cosa succede quando Dynamo viene avviato, quando l'estensione viene caricata e quando Dynamo viene chiuso. Nel file della classe `SampleViewExtension.cs`, aggiungere il seguente codice:

```
using System;
using System.Windows;
using System.Windows.Controls;
using Dynamo.Wpf.Extensions;

namespace SampleViewExtension
{

    public class SampleViewExtension : IViewExtension
    {
        private MenuItem sampleMenuItem;

        public void Dispose()
        {
        }

        public void Startup(ViewStartupParams p)
        {
        }

        public void Loaded(ViewLoadedParams p)
        {
            // Save a reference to your loaded parameters.
            // You'll need these later when you want to use
            // the supplied workspaces

            sampleMenuItem = new MenuItem {Header = "Show View Extension Sample Window"};
            sampleMenuItem.Click += (sender, args) =>
            {
                var viewModel = new SampleWindowViewModel(p);
                var window = new SampleWindow
                {
                    // Set the data context for the main grid in the window.
                    MainGrid = { DataContext = viewModel },

                    // Set the owner of the window to the Dynamo window.
                    Owner = p.DynamoWindow
                };

                window.Left = window.Owner.Left + 400;
                window.Top = window.Owner.Top + 200;

                // Show a modeless window.
                window.Show();
            };
            p.AddMenuItem(MenuBarType.View, sampleMenuItem);
        }

        public void Shutdown()
        {
        }

        public string UniqueId
        {
            get
            {
                return Guid.NewGuid().ToString();
            }  
        } 

        public string Name
        {
            get
            {
                return "Sample View Extension";
            }
        } 

    }
}
```

La classe `SampleViewExtension` crea una voce di menu selezionabile per aprire la finestra e collegarla al modello di vista e alla finestra.

* La classe `public class SampleViewExtension : IViewExtension` `SampleViewExtension` ereditata dall'interfaccia `IViewExtension` fornisce tutto ciò che è necessario per creare la voce di menu.
* `sampleMenuItem = new MenuItem { Header = "Show View Extension Sample Window" };` crea un elemento MenuItem e lo aggiunge al menu `View`.

![La voce di menu](images/dyn-menuitem.jpg)

> 1. La voce di menu

* `sampleMenuItem.Click += (sender, args)` attiva un evento che aprirà una nuova finestra quando si fa clic sulla voce di menu.
* `MainGrid = { DataContext = viewModel }` imposta il contesto dei dati per la griglia principale nella finestra, facendo riferimento a `Main Grid` nel file `.xaml` che creeremo.
* `Owner = p.DynamoWindow` imposta il proprietario della finestra a comparsa su Dynamo. Ciò significa che la nuova finestra dipende da Dynamo, pertanto azioni quali la riduzione a icona, l'ingrandimento e il ripristino di Dynamo faranno sì che la nuova finestra segua lo stesso funzionamento.
* `window.Show();` visualizza la finestra in cui sono state impostate proprietà aggiuntive.

**3\. Implementazione del modello di vista**

Ora che sono stati definiti alcuni parametri di base della finestra, verrà aggiunta la logica per rispondere a vari eventi correlati a Dynamo e verrà richiesto all'interfaccia utente di eseguire l'aggiornamento in base a tali eventi. Copiare il seguente codice nel file della classe `SampleWindowViewModel.cs`:

```
using System;
using Dynamo.Core;
using Dynamo.Extensions;
using Dynamo.Graph.Nodes;

namespace SampleViewExtension
{
    public class SampleWindowViewModel : NotificationObject, IDisposable
    {
        private string activeNodeTypes;
        private ReadyParams readyParams;

        // Displays active nodes in the workspace
        public string ActiveNodeTypes
        {
            get
            {
                activeNodeTypes = getNodeTypes();
                return activeNodeTypes;
            }
        }

        // Helper function that builds string of active nodes
        public string getNodeTypes()
        {
            string output = "Active nodes:\n";

            foreach (NodeModel node in readyParams.CurrentWorkspaceModel.Nodes)
            {
                string nickName = node.Name;
                output += nickName + "\n";
            }

            return output;
        }

        public SampleWindowViewModel(ReadyParams p)
        {
            readyParams = p;
            p.CurrentWorkspaceModel.NodeAdded += CurrentWorkspaceModel_NodesChanged;
            p.CurrentWorkspaceModel.NodeRemoved += CurrentWorkspaceModel_NodesChanged;
        }

        private void CurrentWorkspaceModel_NodesChanged(NodeModel obj)
        {
            RaisePropertyChanged("ActiveNodeTypes");
        }

        public void Dispose()
        {
            readyParams.CurrentWorkspaceModel.NodeAdded -= CurrentWorkspaceModel_NodesChanged;
            readyParams.CurrentWorkspaceModel.NodeRemoved -= CurrentWorkspaceModel_NodesChanged;
        }
    }
}
```

Questa implementazione della classe del modello di vista è in ascolto di `CurrentWorkspaceModel` e attiva un evento quando un nodo viene aggiunto o rimosso dall'area di lavoro. Ciò genera una modifica della proprietà che notifica all'interfaccia utente o agli elementi associati che i dati sono stati modificati e devono essere aggiornati. Viene chiamato il getter `ActiveNodeTypes` che chiama internamente una funzione helper aggiuntiva `getNodeTypes()`. Questa funzione esegue l'iterazione di tutti i nodi attivi nell'area di disegno, compila una stringa contenente i nomi di tali nodi e restituisce questa stringa al nostro binding nel file .xaml da visualizzare nella finestra a comparsa.

Con la logica di base dell'estensione definita, ora specificheremo i dettagli dell'aspetto della finestra con un file `.xaml`. Tutto ciò che serve è una semplice finestra che visualizzi la stringa tramite il binding della proprietà `ActiveNodeTypes` in `TextBlock` `Text`.

![Aggiunta di una finestra](images/vs-window.jpg)

> 1. Fare clic con il pulsante destro del mouse sul progetto e selezionare `Add > New Item...`.
> 2. Selezionare il modello di controllo utente che verrà modificato per creare una finestra.
> 3. Assegnare un nome al nuovo file `SampleWindow.xaml`.
> 4. Selezionare `Add`.

Nel codice della finestra `.xaml`, dovremo eseguire il binding di `SelectedNodesText` ad un blocco di testo. Aggiungere il seguente codice a `SampleWindow.xaml`:

```
<Window x:Class="SampleViewExtension.SampleWindow"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:SampleViewExtension"
             mc:Ignorable="d" 
             d:DesignHeight="300" d:DesignWidth="300"
            Width="500" Height="100">
    <Grid Name="MainGrid" 
          HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch">
        <TextBlock HorizontalAlignment="Stretch" Text="{Binding ActiveNodeTypes}" FontFamily="Arial" Padding="10" FontWeight="Medium" FontSize="18" Background="#2d2d2d" Foreground="White"/>
    </Grid>
</Window>
```

* `Text="{Binding ActiveNodeTypes}"` esegue il binding del valore della proprietà `ActiveNodeTypes` in `SampleWindowViewModel.cs` al valore `TextBlock` `Text` nella finestra.

Ora inizializzeremo la finestra di esempio nel file di backing .xaml C# `SampleWindow.xaml.cs`. Aggiungere il seguente codice a `SampleWindow.xaml`:

```
using System.Windows;

namespace SampleViewExtension
{
    /// <summary>
    /// Interaction logic for SampleWindow.xaml
    /// </summary>
    public partial class SampleWindow : Window
    {
        public SampleWindow()
        {
            InitializeComponent();
        }
    }
}
```

L'estensione della vista è ora pronta per essere creata e aggiunta a Dynamo. Dynamo richiede un file `xml` per registrare il nostro output `.dll` come estensione.

![Aggiunta di un nuovo file XML](images/vs-viewextension-xml.jpg)

> 1. Fare clic con il pulsante destro del mouse sul progetto e selezionare `Add > New Item...`.
> 2. Selezionare il file XML.
> 3. Assegnare un nome al file `SampleViewExtension_ViewExtensionDefinition.xml`.
> 4. Selezionare `Add`.

* Il nome del file segue lo standard di Dynamo per fare riferimento ad un assieme di estensione, come indicato di seguito: `"extensionName"_ViewExtensionDefinition.xml`

Nel file `xml`, aggiungere il seguente codice per indicare a Dynamo dove cercare l'assieme di estensione:

```
<ViewExtensionDefinition>
  <AssemblyPath>C:\Users\username\Documents\Visual Studio 2015\Projects\SampleViewExtension\SampleViewExtension\bin\Debug\SampleViewExtension.dll</AssemblyPath>
  <TypeName>SampleViewExtension.SampleViewExtension</TypeName>
</ViewExtensionDefinition>
```

* In questo esempio, è stato creato l'assieme nella cartella di progetti di default di Visual Studio. Sostituire la destinazione `<AssemblyPath>...</AssemblyPath>` con la posizione dell'assieme.

L'ultimo passaggio consiste nel copiare il file `SampleViewExtension_ViewExtensionDefinition.xml` nella cartella viewExtensions di Dynamo, che si trova nella directory di installazione di Dynamo Core `C:\Program Files\Dynamo\Dynamo Core\1.3\viewExtensions`. È importante notare che sono presenti cartelle separate per `extensions` e `viewExtensions`. Il posizionamento del file `xml` nella cartella errata potrebbe causare errori di caricamento in fase di esecuzione.

![File XML copiato nella cartella viewExtensions](images/fe-viewextension-xml.jpg)

> 1. Il file `.xml` copiato nella cartella viewExtensions di Dynamo

Questa è un'introduzione di base alle estensioni delle viste. Per un case study più sofisticato, vedere il pacchetto DynaShape, un progetto open source su GitHub. Il pacchetto utilizza un'estensione della vista che consente la modifica in tempo reale nella vista modello di Dynamo.

È possibile scaricare un programma di installazione del pacchetto per DynaShape dal forum di Dynamo: [https://forum.dynamobim.com/t/dynashape-published/11666](https://forum.dynamobim.com/t/dynashape-published/11666)

Il codice sorgente può essere clonato da GitHub: [https://github.com/LongNguyenP/DynaShape](https://github.com/LongNguyenP/DynaShape)
