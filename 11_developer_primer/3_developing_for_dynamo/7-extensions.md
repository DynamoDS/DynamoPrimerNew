# Rozšíření 

Rozšíření jsou v ekosystému aplikace Dynamo velmi účinným nástrojem pro vývoj. Umožňují vývojářům vytvářet vlastní funkce založené na interakcích a logice aplikace Dynamo. Rozšíření lze rozdělit na dvě hlavní kategorie: rozšíření a rozšíření pohledu. Jak již název napovídá, systém rozšíření pohledu umožňuje rozšířit uživatelské rozhraní aplikace Dynamo registrací vlastních položek nabídky. Běžná rozšíření fungují velmi podobným způsobem bez uživatelského rozhraní. Můžeme například vytvořit rozšíření, které bude do konzoly aplikace Dynamo zaznamenávat konkrétní informace. Tento scénář nevyžaduje žádné vlastní uživatelské rozhraní, a proto by jej bylo možné provést také pomocí rozšíření.

#### Případová studie rozšíření <a href="#extension-case-study" id="extension-case-study"></a>

S využitím příkladu SampleViewExtension z úložiště DynamoSamples na Githubu projdeme kroky potřebné k vytvoření jednoduchého nemodálního okna, které zobrazuje aktivní uzly v grafu v reálném čase Rozšíření pohledu vyžaduje, abychom pro okno vytvořili uživatelské rozhraní a svázali hodnoty s modelem pohledu.

![Okno rozšíření pohledu](images/dyn-viewextension.jpg)

> 1. Okno rozšíření pohledu vytvořené podle příkladu SampleViewExtension v úložišti na Githubu.

Ačkoliv budeme tento příklad vytvářet od základů, můžete si také stáhnout a vytvořit úložiště DynamoSamples, které vám poslouží jako reference.

Úložiště DynamoSamples: [https://github.com/DynamoDS/DynamoSamples](https://github.com/DynamoDS/DynamoSamples)

> Tato ukázka bude konkrétně odkazovat na projekt s názvem SampleViewExtension, který se nachází v umístění `DynamoSamples/src/`.

#### Implementace rozšíření pohledu <a href="#how-to-implement-a-view-extension" id="how-to-implement-a-view-extension"></a>

Rozšíření pohledu má tři základní součásti:

* Sestava obsahující třídu, která implementuje `IViewExtension`, a třídu, která vytváří model pohledu.
* Soubor `.xml`, který aplikaci Dynamo říká, kde má tuto sestavu za běhu hledat a jaký typ rozšíření má hledat.
* Soubor `.xaml`, který sváže data s grafickým zobrazením a určuje vzhled okna.

**1\. Vytvoření struktury projektu**

Začněte vytvořením nového projektu `Class Library` s názvem `SampleViewExtension`.

![Vytvoření nové knihovny tříd](images/vs-new-project-viewextension-1.jpg)

![Konfigurace nového projektu](images/vs-new-project-viewextension-2.jpg)

> 1. Vytvořte nový projekt výběrem možnosti `File > New > Project`.
> 2. Vyberte `Class Library`.
> 3. Pojmenujte projekt jako `SampleViewExtension`.
> 4. Klikněte na tlačítko `Ok`.

V tomto projektu budeme potřebovat dvě třídy. Jedna třída implementuje `IViewExtension` a druhá implementuje `NotificationObject.`. `IViewExtension` bude obsahovat všechny informace o tom, jak bude rozšíření nasazeno, načteno, odkazováno a zrušeno. `NotificationObject` bude poskytovat oznámení o změnách v aplikaci Dynamo a `IDisposable`. Pokud dojde ke změně, počet se odpovídajícím způsobem aktualizuje.

![Soubory tříd rozšíření pohledu](images/vs-viewextension-classes.jpg)

> 1. Soubor třídy s názvem `SampleViewExtension.cs`, který implementuje `IViewExtension`.
> 2. Soubor třídy s názvem `SampleWindowViewMode.cs`, který implementuje `NotificationObject`.

K použití `IViewExtension` budeme potřebovat balíček NuGet WpfUILibrary. Instalace tohoto balíčku automaticky nainstaluje balíčky Core, Services a ZeroTouchLibrary.

![Balíčky rozšíření pohledu](images/vs-viewextension-packages.jpg)

> 1. Vyberte balíček WpfUILibrary.
> 2. Kliknutím na tlačítko `Install` nainstalujte všechny závislé balíčky.

**2\. Implementace třídy IViewExtension**

Ve třídě `IViewExtension` určíme, co se stane při spuštění aplikace Dynamo, při načtení rozšíření a při ukončení aplikace Dynamo. Do souboru třídy `SampleViewExtension.cs` přidejte následující kód:

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

Třída `SampleViewExtension` vytvoří položku nabídky pro otevření okna, na kterou lze kliknout, a propojí ji k modelu pohledu a oknu.

* `public class SampleViewExtension : IViewExtension` `SampleViewExtension` zdědí z rozhraní `IViewExtension` vše, co je k vytvoření položky nabídky potřeba.
* `sampleMenuItem = new MenuItem { Header = "Show View Extension Sample Window" };` vytvoří objekt MenuItem a přidá jej do nabídky `View`.

![Položka nabídky](images/dyn-menuitem.jpg)

> 1. Položka nabídky

* `sampleMenuItem.Click += (sender, args)` aktivuje událost, která při kliknutí na položku nabídky otevře nové okno.
* `MainGrid = { DataContext = viewModel }` nastaví kontext dat pro hlavní osnovu v okně, s odkazem na `Main Grid` v souboru `.xaml`, který vytvoříme
* `Owner = p.DynamoWindow` nastaví jako vlastníka našeho místního okna aplikaci Dynamo. To znamená, že nové okno je závislé na aplikaci Dynamo, takže akce, například minimalizace, maximalizace a obnovení aplikace Dynamo, způsobí, že nové okno bude mít stejné chování.
* `window.Show();` zobrazí okno, kterému byly nastaveny další vlastnosti okna.

**3\. Implementace modelu pohledu**

Nyní, když jsme zadali některé základní parametry okna, přidáme logiku pro reakci na různé události související s aplikací Dynamo a instruujeme uživatelské rozhraní, aby se aktualizovalo na základě těchto událostí. Do souboru třídy `SampleWindowViewModel.cs` zkopírujte následující kód:

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

Tato implementace třídy modelu pohledu naslouchá `CurrentWorkspaceModel` a aktivuje událost, když je uzel přidán nebo odstraněn z pracovního prostoru. To vyvolá změnu vlastnosti, která upozorní uživatelské rozhraní nebo vázané prvky, že data byla změněna a je nutné je aktualizovat. Je volán getter `ActiveNodeTypes`, který interně volá další pomocnou funkci`getNodeTypes()`. Tato funkce iteruje všechny aktivní uzly na kreslicí ploše, vyplní řetězec obsahující názvy těchto uzlů a vrátí tento řetězec do vazby v souboru XAML, který se zobrazí v našem místním okně.

Po definování základní logiky rozšíření nyní zadáme podrobnosti týkající se vzhledu okna pomocí souboru `.xaml`. Vše, co potřebujeme, je jednoduché okno, které zobrazí řetězec prostřednictvím vazby vlastnosti `ActiveNodeTypes` v `TextBlock` `Text`.

![Přidání okna](images/vs-window.jpg)

> 1. Klikněte pravým tlačítkem na projekt a vyberte `Add > New Item...`.
> 2. Vyberte šablonu User Control, kterou změníme a vytvoříme okno.
> 3. Pojmenujte nový soubor jako `SampleWindow.xaml`.
> 4. Klikněte na tlačítko `Add`.

V kódu `.xaml` okna bude nutné svázat `SelectedNodesText` k textovému bloku. Přidejte následující kód do souboru `SampleWindow.xaml`:

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

* `Text="{Binding ActiveNodeTypes}"` se váže k hodnotě vlastnosti `ActiveNodeTypes` v souboru `SampleWindowViewModel.cs` s hodnotou `TextBlock` `Text`v okně.

Nyní inicializujeme ukázkové okno v záložním souboru XAML C# `SampleWindow.xaml.cs`. Přidejte následující kód do souboru `SampleWindow.xaml`:

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

Rozšíření pohledu je nyní připraveno k sestavení a přidání do aplikace Dynamo. Aplikace Dynamo vyžaduje soubor `xml`, aby bylo možné zaregistrovat výstup `.dll` jako rozšíření.

![Přidání nového souboru XML](images/vs-viewextension-xml.jpg)

> 1. Klikněte pravým tlačítkem na projekt a vyberte `Add > New Item...`.
> 2. Vyberte soubor XML.
> 3. Pojmenujte soubor jako `SampleViewExtension_ViewExtensionDefinition.xml`.
> 4. Klikněte na tlačítko `Add`.

* Název souboru se řídí standardem aplikace Dynamo pro odkazování na sestavu rozšíření takto: `"extensionName"_ViewExtensionDefinition.xml`

V souboru `xml` přidejte následující kód, který aplikaci Dynamo řekne, kde má hledat sestavu rozšíření:

```
<ViewExtensionDefinition>
  <AssemblyPath>C:\Users\username\Documents\Visual Studio 2015\Projects\SampleViewExtension\SampleViewExtension\bin\Debug\SampleViewExtension.dll</AssemblyPath>
  <TypeName>SampleViewExtension.SampleViewExtension</TypeName>
</ViewExtensionDefinition>
```

* V tomto příkladu jsme sestavu vytvořili do výchozí složky projektu aplikace Visual Studio. Nahraďte cíl `<AssemblyPath>...</AssemblyPath>` umístěním sestavy.

Posledním krokem je zkopírování souboru `SampleViewExtension_ViewExtensionDefinition.xml` do složky rozšíření pohledů aplikace Dynamo, která se nachází v instalačním adresáři jádra aplikace Dynamo `C:\Program Files\Dynamo\Dynamo Core\1.3\viewExtensions`. Je důležité si uvědomit, že existují samostatné složky pro `extensions` a `viewExtensions`. Umístění souboru `xml` do nesprávné složky může způsobit nesprávné načtení za běhu.

![Soubor XML zkopírovaný do složky rozšíření](images/fe-viewextension-xml.jpg)

> 1. Soubor `.xml`, který jsme zkopírovali do složky rozšíření pohledů aplikace Dynamo.

Toto je základní úvod k rozšířením pohledu. Propracovanější případovou studii naleznete v balíčku DynaShape, projektu s otevřeným zdrojovým kódem na Githubu. Tento balíček používá rozšíření pohledu, které umožňuje živé úpravy v pohledu modelu aplikace Dynamo.

Instalační program balíčku DynaShape lze stáhnout z fóra aplikace Dynamo: [https://forum.dynamobim.com/t/dynashape-published/11666](https://forum.dynamobim.com/t/dynashape-published/11666)

Zdrojový kód lze klonovat z Githubu: [https://github.com/LongNguyenP/DynaShape](https://github.com/LongNguyenP/DynaShape)
