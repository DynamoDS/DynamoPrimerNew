# Język Python i program Civil 3D

Dodatek Dynamo daje niezwykłe możliwości jako narzędzie do [programowania wizualnego](../../a\_appendix/a-1\_visual-programming-and-dynamo.md). Można jednak pominąć węzły i przewody, aby pisać kod w postaci tekstowej. Można to zrobić na dwa sposoby:

1. Pisanie kodu **DesignScript** za pomocą węzła Code Block
2. Pisanie kodu **Python** za pomocą węzła Python

W tej sekcji omówiono używanie języka Python w środowisku programu Civil 3D w celu wykorzystywania interfejsów API .NET programów AutoCAD i Civil 3D.

{% hint style="info" %} Aby uzyskać bardziej ogólne informacje na temat używania języka Python w dodatku Dynamo, skorzystaj z sekcji [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention"). {% endhint %}

## Dokumentacja interfejsu API

Dla programów AutoCAD i Civil 3D jest dostępnych po kilka interfejsów API, które umożliwiają programistom rozszerzanie produktu podstawowego o funkcje niestandardowe. W kontekście dodatku Dynamo istotne są **interfejsy API kodu zarządzanego .NET**. Poniższe łącza prowadzą do informacji niezbędnych do zrozumienia struktury tych interfejsów API i sposobu ich działania.

[Podręcznik programisty interfejsu API .NET dla programu AutoCAD](https://help.autodesk.com/view/OARX/2024/PLK/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Podręcznik użytkownika interfejsu API .NET dla programu AutoCAD](https://help.autodesk.com/view/OARX/2024/PLK/?guid=OARX-ManagedRefGuide-What_s_New)

[Podręcznik programisty interfejsu API .NET dla programu Civil 3D](https://help.autodesk.com/view/CIV3D/2024/PLK/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Podręcznik użytkownika interfejsu API .NET dla programu Civil 3D](https://help.autodesk.com/view/CIV3D/2024/PLK/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} Podczas zapoznawania się z tą sekcją możesz zetknąć się z pewnymi nowymi dla Ciebie pojęciami, takimi jak bazy danych, transakcje, metody, właściwości itp. Wiele z tych pojęć należy do podstaw pracy z interfejsami API .NET i nie są one charakterystyczne ani dla dodatku Dynamo, ani dla języka Python. Szczegółowe omówienie tych elementów wykracza poza zakres tej sekcji przewodnika Primer, dlatego zaleca się częste korzystanie z informacji, do których prowadzą powyższe łącza. {% endhint %}

## Szablon kodu

Podczas pierwszej edycji nowego węzła w języku Python jest on wstępnie wypełniany kodem-szablonem, aby przyspieszyć rozpoczęcie pracy. Oto podział szablonu z objaśnieniami dotyczącymi każdego bloku.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Domyślny szablon węzła w języku Python w programie Civil 3D</p></figcaption></figure>

> 1. Importuje moduły `sys` i `clr`, które są niezbędne do poprawnego działania interpretera języka Python. W szczególności moduł `clr` umożliwia traktowanie przestrzeni nazw .NET jako pakietów Python.
> 2. Wczytuje standardowe zespoły (np. pliki DLL) do pracy z interfejsami API kodu zarządzanego .NET dla programów AutoCAD i Civil 3D.
> 3. Dodaje odniesienia do standardowych przestrzeni nazw programów AutoCAD i Civil 3D. Są one równoważne z dyrektywami `using` lub `Imports` odpowiednio w języku C# i w języku VB.NET.
> 4. Dostęp do portów wejściowych węzła można uzyskać za pomocą wstępnie zdefiniowanej listy o nazwie `IN`. Dostęp do danych w określonym porcie można uzyskać, używając numeru indeksu, na przykład `dataInFirstPort = IN[0]`.
> 5. Pobiera aktywny dokument i edytor.
> 6. Blokuje dokument i inicjuje transakcję bazy danych.
> 7. W tym miejscu należy umieścić większość kodu logiki skryptu.
> 8. Usuń oznaczenie komentarza tego wiersza, aby zatwierdzić transakcję po zakończeniu głównej pracy.
> 9. Aby zapisać dane wyjściowe węzła, należy przypisać je do zmiennej `OUT` na końcu skryptu.

{% hint style="info" %} **Chcesz wprowadzić dostosowania?**\
 Domyślny szablon w języku Python można zmodyfikować, edytując plik `PythonTemplate.py` znajdujący się w folderze `C:\ProgramData\Autodesk\C3D <version>\Dynamo`. {% endhint %}

## Przykład

Przeanalizujmy przykład, aby zademonstrować niektóre z najważniejszych pojęć dotyczących pisania skryptów w języku Python w dodatku Dynamo dla programu Civil 3D.

### Cel

> :dart: Pobieranie geometrii obwiedni wszystkich zlewni na rysunku.

### Zestaw danych

Poniżej przedstawiono przykładowe pliki, z których można korzystać w trakcie tego ćwiczenia.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Przegląd rozwiązania

Poniżej przedstawiono przegląd logiki na tym wykresie.

> 1. Zapoznaj się z dokumentacją interfejsu API programu Civil 3D
> 2. Wybranie wszystkich zlewni w dokumencie na podstawie nazwy warstwy
> 3. „Odpakowanie” obiektów Dynamo, aby uzyskać dostęp do wewnętrznych składników interfejsu API programu Civil 3D
> 4. Utworzenie punktów dodatku Dynamo na podstawie punktów programu AutoCAD
> 5. Utworzenie krzywych PolyCurve z punktów

Zacznijmy!

### Przegląd dokumentacji interfejsu API

Przed rozpoczęciem tworzenia wykresu i pisania kodu warto zapoznać się z dokumentacją interfejsu API programu Civil 3D i dowiedzieć się, co udostępnia ten interfejs API. W tym przypadku istnieje [właściwość w klasie zlewni (Catchment)](https://help.autodesk.com/view/CIV3D/2024/PLK/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c), która zwraca punkty obwiedni zlewni. Ta właściwość zwraca obiekt `Point3dCollection`, którego dodatek Dynamo nie jest w stanie domyślnie obsługiwać. Oznacza to, że nie można utworzyć krzywej PolyCurve z obiektu `Point3dCollection`, więc konieczne będzie przekształcenie wszystkiego w punkty dodatku Dynamo. Więcej informacji na ten temat podamy w dalszej części.

### Pobranie wszystkich zlewni

Teraz możemy zacząć tworzyć logikę wykresu. Pierwszą czynnością, którą należy wykonać, jest pobranie listy wszystkich zlewni w dokumencie. Dostępne są węzły do obsługi tej operacji, więc nie trzeba uwzględniać jej w skrypcie w języku Python. Używanie węzłów zapewnia lepszą przejrzystość dla innych osób czytających wykres (w przeciwieństwie do używania dużej ilości kodu w skrypcie w języku Python) i pozwala skupić się w kodzie w języku Python na jednej rzeczy: zwróceniu punktów obwiedni zlewni.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Pobieranie wszystkich zlewni w dokumencie na podstawie warstwy</p></figcaption></figure>

Warto zwrócić uwagę, że wyjście z węzła **All Objects on Layer** jest listą obiektów programu Civil (CivilObject). Jest to spowodowane tym, że dodatek Dynamo for Civil 3D nie zawiera obecnie żadnych węzłów do pracy ze zlewniami. Dlatego właśnie należy uzyskać dostęp do interfejsu API za pośrednictwem języka Python.

### Odpakowywanie obiektów

Zanim przejdziemy dalej, musimy krótko odnieść się do ważnego pojęcia. W sekcji [node-library.md](../node-library.md "mention") omówiono powiązania obiektów i obiektów programu Civil (CivilObject). Bardziej szczegółowo można powiedzieć, że **obiekt Dynamo** jest opakowaniem dla **elementu programu AutoCAD**. Podobnie **obiekt programu Civil w dodatku Dynamo (CivilObject)** jest opakowaniem dla **elementu programu Civil 3D**. Można „odpakować” obiekt, uzyskując dostęp do jego właściwości `InternalDBObject` lub `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Typ dodatku Dynamo</th><th width="373">Opakowania</th></tr></thead><tbody><tr><td><strong>Obiekt</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Element</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Element</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} Ogólnie bezpieczniej jest uzyskać identyfikator obiektu za pomocą właściwości `InternalObjectId`, a następnie uzyskać dostęp do opakowanego obiektu w transakcji. Wynika to z tego, że właściwość `InternalDBObject` zwraca obiekt DBObject programu AutoCAD, który nie jest w stanie zapisywalnym. {% endhint %}

### Skrypt w języku Python

Oto pełny skrypt w języku Python, który wykonuje operacje polegające na uzyskaniu dostępu do wewnętrznych obiektów zlewni i pobraniu ich punktów obwiedni. Wyróżnione wiersze to te zmodyfikowane lub dodane w domyślnym kodzie-szablonie.

{% hint style="info" %} Klikaj podkreślony tekst w skrypcie, aby uzyskać wyjaśnienia dotyczące poszczególnych wierszy. {% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Wczytywanie bibliotek standardowych języka Python i bibliotek języka DesignScript
import sys
import clr

# Dodawanie zespołów dla programów AutoCAD i Civil 3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Importowanie odniesień z programu AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Importowanie odniesień z programu Civil 3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Dane wejściowe tego węzła będą przechowywane jako lista w zmiennych IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("Dane wejściowe są puste lub mają wartość null.")</a>
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
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Zatwierdzenie przed zakończeniem transakcji
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Przypisanie danych wyjściowych do zmiennej OUT.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

Ogólnie najlepszą praktyką jest umieszczenie większości kodu logiki skryptu wewnątrz transakcji. Zapewnia to bezpieczny dostęp do obiektów, które są odczytywane/zapisywane przez skrypt. W wielu przypadkach pominięcie transakcji może spowodować błąd krytyczny. {% endhint %}

### Tworzenie krzywych PolyCurve

Na tym etapie skrypt w języku Python powinien zwrócić listę punktów dodatku Dynamo, które można wyświetlić w podglądzie w tle. Ostatnią czynnością jest utworzenie krzywych PolyCurve na podstawie punktów. Warto zauważyć, że można to również zrobić bezpośrednio w skrypcie w języku Python. Jednak celowo umieściliśmy tę operację poza skryptem w węźle, aby była bardziej widoczna. Oto ostateczny wykres.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Ostateczny wykres</p></figcaption></figure>

### Wynik

Oto ostateczna geometria dodatku Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Wynikowe krzywe PolyCurve dodatku Dynamo dla obwiedni zlewni</p></figcaption></figure>

> :tada: Misja wykonana!

## IronPython a CPython

Zanim zakończymy tę część, omówmy jeszcze jedną kwestię. W zależności od używanej wersji programu Civil 3D węzeł w języku Python może być skonfigurowany w określony sposób. W programach **Civil 3D 2020 i 2021** dodatek Dynamo używał narzędzia o nazwie **IronPython** do przenoszenia danych między obiektami .NET a skryptami w języku Python. Jednak w programie **Civil 3D 2022** dodatek Dynamo używa standardowego natywnego interpretera języka Python (znanego jako **CPython**), w którym jest używany język Python 3. Korzyści płynące z przejścia na nowy model obejmują dostęp do popularnych nowoczesnych bibliotek i nowych funkcji platformy, niezbędne poprawki konserwacyjne i poprawki zabezpieczeń.

{% hint style="info" %} Więcej informacji na temat tego przejścia i uaktualniania starszych skryptów można znaleźć w [blogu dotyczącym dodatku Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Aby nadal używać mechanizmu IronPython, wystarczy zainstalować pakiet **DynamoIronPython2.7** za pomocą Menedżera pakietów Dynamo. {% endhint %}

[^1]: Domyślnie biblioteka geometrii dodatku Dynamo nie jest dodawana do środowiska języka Python. Celem tego skryptu jest utworzenie listy punktów dodatku Dynamo dla obwiedni zlewni, dlatego należy dodać ten wiersz, aby utworzyć punkty później.

[^2]: Ten wiersz pobiera określoną potrzebną klasę z biblioteki geometrii dodatku Dynamo. Uwaga: określono tutaj `import Point as DynPoint` zamiast `import *`, ponieważ ta druga opcja spowodowałaby kolizje nazw.

[^3]: Zmieniamy tu nazwę zmiennej domyślnej `dataEnteringNode` na `objs`, dzięki czemu jest ona bardziej opisowa.

[^4]: Tutaj dokładnie określamy, który port wejściowy zawiera dane, jakie mają być używane, zamiast domyślnych `IN`, które odnoszą się do całej listy wszystkich danych wejściowych.

[^5]: Tworzy pustą listę, do której później dodamy dane wyjściowe.

[^6]: Ma to na celu zabezpieczenie przed ewentualnymi pustymi danymi wejściowymi skryptu lub danymi wejściowymi o wartości null.

[^7]: Zamiast przerywać skrypt, zostanie zwrócony komunikat z wyjaśnieniem, dlaczego nie można kontynuować skryptu.

[^8]: W pozostałej części skryptu założono, że używana jest lista obiektów (a nie pojedynczy obiekt). Jednak w przypadku gdyby istniał tylko jeden obiekt (np. rysunek z pojedynczą zlewnią), skrypt powinien nadal zostać uruchomiony.

[^9]: Jeśli dane wejściowe nie są listą (tj. są pojedynczym obiektem), po prostu utworzymy nową listę, której jedynym elementem jest ten obiekt.

[^10]: Zainicjowanie pętli dla każdego obiektu na liście.

[^11]: „Odpakowanie” obiektu Dynamo przez pobranie jego identyfikatora obiektu.

[^12]: Pobranie „opakowanego” obiektu z bazy danych programu AutoCAD. W tym miejscu tryb OpenMode jest ustawiony na `ForRead`, ponieważ nie planujemy wprowadzania zmian w obiektach. Upraszczamy „dane zapytania”.

[^13]: Możliwe, że lista wejściowa obiektów zawiera kombinację zlewni i innych elementów innych niż zlewnie. Musimy sprawdzić, czy nie występuje taka sytuacja, i odpowiednio ją obsłużyć (tzn. kontynuować tę iterację pętli tylko wtedy, gdy element jest zlewnią).

[^14]: Jeśli skrypt dojdzie do tego etapu, wiemy, że obiekt jest zlewnią. Dodamy tutaj nową zmienną, aby zachować przejrzystość i zrozumiałość nazewnictwa.

[^15]: W tym miejscu pobieramy punkty obwiedni zlewni za pomocą odpowiedniej właściwości, którą wyszukaliśmy wcześniej w dokumentacji interfejsu API. Jak wspomniano wcześniej, uzyskamy w ten sposób obiekt `Point3dCollection`, który jest zasadniczo listą punktów programu AutoCAD. Musimy „przekształcić” go w punkty dodatku Dynamo, aby móc je wykorzystać.

[^16]: Utworzenie pustej listy, aby zapisać punkty obwiedni dla tej zlewni.

[^17]: Zainicjowanie pętli dla każdego obiektu `Point3d` w kolekcji `Point3dCollection`.

[^18]: Utworzenie punktu dodatku Dynamo za pomocą współrzędnych punktu programu AutoCAD.

[^19]: Dodanie punktu do listy.

[^20]: Po zakończeniu „przekształcania” wszystkich punktów programu AutoCAD w punkty dodatku Dynamo dodajemy listę wynikową do danych wyjściowych. Następnie pętla zewnętrzna przejdzie do następnego obiektu na liście wejściowej.

[^21]: Usuń oznaczenie komentarza tego wiersza, aby zatwierdzić transakcję.

[^22]: Na koniec zwracamy jako dane wyjściowe listę punktów dodatku Dynamo.
