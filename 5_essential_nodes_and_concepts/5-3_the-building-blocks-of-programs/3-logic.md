# Logika

**Logika**, a w szczególności **logika warunkowa**, pozwala określić operację lub zestaw operacji na podstawie testu. Przez oszacowanie testu uzyskamy wartość logiczną reprezentującą prawdę (`True`) lub fałsz (`False`), za pomocą której można sterować przepływem programu.

### Wartości logiczne

Zmienne liczbowe mogą przechowywać liczby z szerokiego zakresu. Zmienne logiczne mogą przechowywać tylko dwie wartości, nazywane Prawda lub Fałsz, Tak lub Nie albo 1 lub 0. Rzadko stosuje się operacje logiczne do wykonywania obliczeń z powodu ich ograniczonego zakresu.

### Instrukcje warunkowe

Instrukcja „If” (jeśli) stanowi kluczowe pojęcie w programowaniu: „Jeśli _to_ jest prawdą, wtedy _tak_ się stanie, w przeciwnym razie stanie się _coś innego_”. Wynikowa operacja tej instrukcji zależy od wartości logicznej. Istnieje wiele sposobów definiowania instrukcji „If” w dodatku Dynamo:

| Ikona                                             | Nazwa (składnia)             | Dane wejściowe            | Dane wyjściowe |
| ------------------------------------------------ | ------------------------- | ----------------- | ------- |
| ![](../images/5-1/If.jpg)                        | Jeśli (**If**)               | test, prawda, fałsz | wynik  |
| \![](<../images/5-1/CodeBlock(1)(1) (1) (1).jpg>) | Code Block (**(x?y:z);**) | x? y, z           | wynik  |

Przeanalizujmy krótki przykład dotyczący działania każdego z tych trzech węzłów z użyciem instrukcji warunkowej „If”.

Na tej ilustracji _wartość logiczna_ jest ustawiona na _true_, co oznacza, że wynik jest ciągiem: _„this is the result if true”_ (to jest wynik, jeśli prawda). Trzy węzły tworzące instrukcję _If_ działają tu w ten sam sposób.

![](../images/5-3/3/logic-conditionalstatements01false.jpg)

Węzły działają identycznie. Jeśli _wartość logiczna_ zostanie zmieniona na _false_, wynik będzie liczbą _Pi_, jak to zdefiniowano w oryginalnej instrukcji _If_.

![](../images/5-3/3/logic-conditionalstatements02true.jpg)

## Ćwiczenie: logika i geometria

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-3/3/Building Blocks of Programs - Logic.dyn" %}

### Część I: filtrowanie listy

1. Użyjmy logiki, aby rozdzielić listę liczb na listę liczb parzystych i listę liczb nieparzystych.

![](../images/5-3/3/logic-exercisepartI-01.jpg)

> a. **Number Range —** dodaj zakres liczb do obszaru rysunku.
>
> b. **Number —** dodaj trzy number liczb do obszaru rysunku. Wartość dla każdego węzła number powinna wynosić: _0,0_ dla _start_, _10,0_ dla _end_ i _1,0_ dla _step_.
>
> c. **Wyjście** — wynik wyjściowy to lista 11 liczb w zakresie od 0 do 10.
>
> d. **Modulo (%) —** węzeł **Number Range** do _x_ i _2,0_ do _y_. Spowoduje to obliczenie reszty z dzielenia przez 2 dla każdej liczby na liście. Wynik z tej listy to lista wartości 0 i 1.
>
> e. **Test równości (==) —** dodaj test równości do obszaru rysunku. Podłącz wyjście _modulo_ do wejścia _x_ i wartość _0,0_ do wejścia _y_.
>
> f. **Watch —** wynik testu równości jest listą wartości logicznych true i false. Są to wartości używane do oddzielenia elementów na liście. _0_ (lub _true_) reprezentuje liczby parzyste, a _1_ (lub _false_) reprezentuje liczby nieparzyste.
>
> g. **List.FilterByBoolMask —** ten węzeł filtruje wartości na dwie różne listy w oparciu o wejściową wartość logiczną. Podłącz oryginalny węzeł _Number Range_ do wejścia _list_ oraz wyjście _equality test_ do wejścia _mask_. Wyjście _in_ reprezentuje wartości true, podczas gdy wyjście _out_ reprezentuje wartości false.
>
> h. **Watch** — w wyniku tego mamy teraz listę liczb parzystych i listę liczb nieparzystych. Użyliśmy operatorów logicznych do rozdzielenia list na wzory.

### Część II: od logiki do geometrii

Bazując na logice ustanowionej w pierwszym ćwiczeniu, zastosujmy tę konfigurację do operacji modelowania.

2\. Oprzemy się na poprzednim ćwiczeniu z tymi samymi węzłami. Jedyne wyjątki to (oprócz zmiany formatu):

![](../images/5-3/3/logic-exercisepartII-01.jpg)

> a. Użyj węzła **Sequence** z tymi wartościami wejściowymi.
>
> b. Odłączyliśmy wejście list in od węzła **List.FilterByBoolMask**. Na razie odłożymy te węzły na bok, ale później w tym ćwiczeniu się przydadzą.

3\. Zacznijmy od utworzenia oddzielnej grupy wykresu, jak pokazano na ilustracji powyżej. Ta grupa węzłów reprezentuje równanie parametryczne definiujące krzywą liniową. Kilka uwag:

![](../images/5-3/3/logic-exercisepartII-02.jpg)

> a. Pierwszy suwak **Number Slider** reprezentuje częstotliwość fali. Powinien mieć wartość min. 1, maks. 4 i krok 0,01.
>
> b. Drugi **Number Slider** reprezentuje amplitudę fali. Powinien mieć wartość min. 0, maks. 1 i krok równy 0,01.
>
> c. **PolyCurve.ByPoints —** jeśli zostanie skopiowany powyższy wykres węzłów, wynikiem będzie krzywa sinusoidalna w rzutni podglądu Dynamo.

Metoda stosowana tutaj dla wejść: użyj węzłów number dla bardziej statycznych właściwości i węzłów Number Slider dla właściwości bardziej elastycznych. Chcemy zachować oryginalny węzeł Number Range definiowany na początku tego kroku. Jednak tworzona tutaj krzywa sinusoidalna powinna mieć pewną elastyczność. Możemy przesunąć te suwaki, aby obserwować, jak aktualizowane są częstotliwość i amplituda krzywej.

![](../images/5-3/3/logic-exercisepartII-03.gif)

4\. Będziemy analizować definicję nie po kolei, więc spójrzmy na wynik końcowy, aby móc się odwoływać do tego, do czego dążymy. Pierwsze dwa kroki są wykonywane oddzielnie, chcemy je teraz połączyć. Użyjemy bazowej krzywej sinusoidalnej do sterowania położeniem komponentów zamka, a za pomocą logiki prawdy/fałszu będziemy przełączać się między małymi i większymi kostkami.

![](../images/5-3/3/logic-exercisepartII-04.jpg)

> a. **Math.RemapRange** — za pomocą sekwencji liczb utworzonej w kroku 02 utwórzmy nową serię liczb poprzez ponowne odwzorowanie zakresu. Oryginalne liczby z zakresu od 0 do 100 z kroku 01. Te liczby mieszczą się w zakresie od 0 do 1 dla odpowiednio wejść _newMin_ i _newMax_.

5\. Utwórz węzeł **Curve.PointAtParameter**, a następnie połącz wyjście **Math.RemapRange** z kroku 04 jako wejście _param_.

![](../images/5-3/3/logic-exercisepartII-05.jpg)

W tym kroku tworzone są punkty wzdłuż krzywej. Ponownie odwzorowaliśmy liczby na wartości od 0 do 1, ponieważ wejście _param_ szuka wartości w tym zakresie. Wartość _0_ reprezentuje punkt początkowy, a wartość _1_ reprezentuje punkty końcowe. Wszystkie liczby między nimi są odwzorowane na wartości z zakresu _[0,1]_.

6\. Połącz wyjście z węzła **Curve.PointAtParameter** z węzłem **List.FilterByBoolMask**, aby rozdzielić listę indeksów nieparzystych i nieparzystych.

![](../images/5-3/3/logic-exercisepartII-06.jpg)

> a. **List.FilterByBoolMask** — podłącz węzeł **Curve.PointAtParameter** z poprzedniego kroku do wejścia _list_.
>
> b. **Watch —** węzeł obserwacyjny dla _in_ i węzeł obserwacyjny dla _out_ pokazują, że mamy dwie listy reprezentujące indeksy parzyste i nieparzyste. Punkty te są uporządkowane w ten sam sposób na krzywej, co przedstawimy w następnym kroku.

7\. Następnie użyjemy wyjścia z węzła **List.FilterByBoolMask** w kroku 05, aby wygenerować geometrie o rozmiarach zgodnych z indeksami.

**Cuboid.ByLengths —** ponownie utwórz połączenia widoczne na ilustracji powyżej, aby uzyskać zamek wzdłuż krzywej sinusoidalnej. Prostopadłościan jest tutaj tylko kostką. Definiujemy jego rozmiar na podstawie punktu krzywej w środku kostki. Logika podziału na wartości parzyste/nieparzyste powinna być teraz czytelna w modelu.

![](../images/5-3/3/logic-exercisepartII-07.jpg)

> a. Lista prostopadłościanów przy indeksach parzystych.
>
> b. Lista prostopadłościanów przy indeksach nieparzystych.

Gotowe! Właśnie zaprogramowano proces definiowania wymiarów geometrii zgodnie z operacją logiczną przedstawioną w tym ćwiczeniu.
