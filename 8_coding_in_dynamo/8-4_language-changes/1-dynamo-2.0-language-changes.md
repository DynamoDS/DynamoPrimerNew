# Zmiany języka

Sekcja Zmiany języka zawiera przegląd aktualizacji i modyfikacji wprowadzonych w dodatku Dynamo w poszczególnych wersjach. Te zmiany mogą mieć wpływ na funkcjonalność, wydajność i użycie — ten przewodnik pomoże użytkownikom zrozumieć, kiedy i dlaczego należy dostosować się do tych aktualizacji.

## Zmiany dotyczące języka dodatku Dynamo 2.0

1. Zmiana składni lista@poziom z „@-1” na „@L1” (L— „level”, czyli „poziom”)
* Nowa składnia dla lista@poziom pozwalająca używać zapisu lista@L1 zamiast lista@-1
* Motywacja: dostosowanie składni kodu do podglądu/interfejsu użytkownika; z testów z udziałem użytkowników wynika, że ta nowa składnia jest bardziej zrozumiała

2. Zaimplementowanie typów Int i Double w TS w celu dostosowania do typów dodatku Dynamo

3. Niezezwalanie na przeciążenia funkcji, w których argumenty różnią się tylko kardynalnością
* Stare wykresy, w których używa się usuniętych przeciążeń, powinny domyślnie używać przeciążeń o wyższej randze.
* Motywacja: wyeliminowanie niejasności co do tego, która konkretnie funkcja jest wykonywana

4. Wyłączenie podwyższania poziomu szyku (tablicy) za pomocą prowadnic replikacji

5. Ustawienie zmiennych w blokach imperatywnych jako lokalnych w zakresie bloku imperatywnego
* Wartości zmiennych zdefiniowane wewnątrz imperatywnych bloków kodu nie zostaną zmodyfikowane przez zmiany wewnątrz bloków imperatywnych, które się do nich odnoszą.  

6. Ustawienie zmiennych jako niemodyfikowalnych w celu wyłączenia aktualizacji asocjacyjnej w węzłach bloku kodu

7. Kompilowanie wszystkich węzłów interfejsu użytkownika do metod statycznych

8. Obsługa instrukcji return bez przypisania
* Używanie znaku „=” nie jest konieczne ani w definicjach funkcji, ani w kodzie imperatywnym.

9. Migracja starych nazw metod w węzłach bloku kodu
* Nazwy wielu węzłów zostały zmienione w celu zwiększenia czytelności i umieszczenia w interfejsie użytkownika Przeglądarki biblioteki.

10. Oczyszczenie list prezentowanych jako słowniki

----
Znane problemy:
- Konflikty przestrzeni nazw w blokach imperatywnych powodują pojawienie się nieoczekiwanych portów wejściowych. Zobacz temat [Problem z serwisem Github](https://github.com/DynamoDS/Dynamo/issues/8796), aby uzyskać więcej informacji. Aby obejść ten problem, zdefiniuj funkcję poza blokiem imperatywnym w następujący sposób:
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Wyjaśnienie zmian dotyczących języka w dodatku Dynamo 2.0

W języku w dodatku Dynamo 2.0 wprowadzono szereg ulepszeń. Główną motywacją do wprowadzenia tych zmian było uproszczenie języka. Nacisk położono na uczynienie języka DesignScript bardziej zrozumiałym i prostszym w użyciu na potrzeby zwiększenia jego wydajności i elastyczności w celu poprawy zrozumiałości dla użytkownika końcowego.

Poniżej znajduje się lista zmian w wersji 2.0 wraz z objaśnieniami:
* Uproszczono składnię lista@poziom
* Przeciążane metody z parametrami, które różnią się tylko rangą, są niedozwolone 
* Kompilowanie wszystkich węzłów interfejsu użytkownika jako metod statycznych
* Wyłączono podwyższanie poziomu do listy w przypadku używania z prowadnicami replikacji/skratowaniem
* Zmienne w blokach asocjacyjnych są niemodyfikowalne, aby zapobiec aktualizacji asocjacyjnej
* Zmienne w blokach imperatywnych są lokalne w zakresie imperatywnym
* Oddzielenie list i słowników

## 1\. Uproszczono składnię lista@poziom 

Nowa składnia dla lista@poziom pozwalająca używać zapisu `list@L1` (lista@L1) zamiast `list@-1` (lista@-1) ![](../images/8-4/1/lang2_1.png)


## 2\. Przeciążane funkcje z parametrami, które różnią się tylko rangą, są niedozwolone
Przeciążane funkcje są problematyczne z wielu powodów:
* Przeciążana funkcja wskazywana przez węzeł interfejsu użytkownika na wykresie może nie być tym samym przeciążeniem, które jest używane w czasie wykonywania
* Rozpoznawanie metod jest kosztowne i nie działa dobrze w przypadku przeciążanych funkcji
* Trudno jest zrozumieć zachowanie replikacji w przypadku przeciążanych funkcji

Rozważmy przykład `BoundingBox.ByGeometry`: w starszych wersjach dodatku Dynamo istniały dwie przeciążane funkcje — jedna, która przyjmowała pojedynczy argument wartości, i druga, która przyjmowała jako argument listę geometrii:
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Jeśli użytkownik upuścił pierwszy węzeł w obszarze rysunku i połączył z nim listę geometrii, będzie oczekiwał uruchomienia replikacji, ale nigdy tak się nie stanie, ponieważ w czasie wykonywania zostanie wywołane drugie przeciążenie, jak pokazano na ilustracji: ![](../images/8-4/1/lang2_2.png)
 
Z tego powodu w wersji 2.0 nie zezwalamy na przeciążane funkcje, które różnią się tylko kardynalnością parametrów. Oznacza to, że w przypadku przeciążanych funkcji, które mają tę samą liczbę i typy parametrów, ale mają co najmniej jeden parametr, który różni się tylko rangą, przeciążenie, które jest zdefiniowane jako pierwsze, zawsze wygrywa, podczas gdy reszta jest odrzucana przez kompilator. Główną zaletą tego uproszczenia jest uproszczenie logiki rozpoznawania metod dzięki szybkiej ścieżce wyboru kandydujących funkcji.

W bibliotece geometrii dla wersji 2.0 pierwsze przeciążenie w przykładzie `BoundingBox.ByGeometry` zostało wycofane, a drugie zostało zachowane, więc jeśli węzeł ma wykonywać replikację, czyli jest używany w kontekście pierwszego przeciążenia, należy go użyć z najkrótszą (lub najdłuższą) opcją skratowania albo w bloku kodu z prowadnicami replikacji: 
```
BoundingBox.ByGeometry(geometry<1>);
```
W tym przykładzie widzimy, że węzeł o wyższej randze może być używany zarówno w wywołaniu z replikacją, jak i bez replikacji i dlatego zawsze jest preferowany w stosunku do przeciążenia o niższej randze. Z tego powodu autorom węzłów **zawsze zaleca się rezygnację z przeciążeń o niższej randze na rzecz metod o wyższej randze**, tak aby kompilator języka DesignScript zawsze wywoływał metodę o wyższej randze jako pierwszą i jedyną, którą znajdzie.

### Przykłady:
W poniższym przykładzie zdefiniowano dwa przeciążenia funkcji `foo`. W wersji 1.x to, które przeciążenie jest używane w czasie wykonywania, jest niejednoznaczne. Użytkownik może oczekiwać, że zostanie wykonane drugie przeciążenie `foo(a:int, b:int)`. W takim przypadku oczekuje się, że metoda wykona replikację trzy razy, zwracając wartość `10` trzy razy. W rzeczywistości zwracana jest pojedyncza wartość `10`, ponieważ wbrew oczekiwaniom wywoływane jest pierwsze przeciążenie z parametrem list.

### Drugie przeciążenie jest pomijane w wersji 2.0:
W wersji 2.0 zawsze wybierana jest pierwsza zdefiniowana metoda. Zastosowanie ma zasada „kto pierwszy, ten lepszy”.

![](../images/8-4/1/lang2_3.png)

W każdym z poniższych przypadków zostanie przyjęte pierwsze zdefiniowane przeciążenie. Należy pamiętać, że wybór opiera się wyłącznie na kolejności definiowania funkcji, a nie na rangach parametrów, chociaż zaleca się preferowanie metod o parametrach z wyższymi rangami dla węzłów zdefiniowanych przez użytkownika i węzłów Zero Touch.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Kompilowanie wszystkich węzłów interfejsu użytkownika do metod statycznych
W dodatku Dynamo 1.x węzły interfejsu użytkownika (węzły inne niż bloki kodu) były kompilowane odpowiednio do postaci metod i właściwości wystąpienia. Na przykład węzeł `Point.X` był kompilowany do `pt.X`, a węzeł `Curve.PointAtParameter` do `curve.PointAtParameter(param)`. Z tym zachowaniem wiązały się dwa problemy:

__A. Funkcja reprezentowana przez węzeł interfejsu użytkownika nie zawsze była tą samą funkcją, która była wykonywana w czasie wykonywania__

Typowym przykładem jest węzeł `Translate`. Istnieje wiele węzłów `Translate`, które przyjmują tę samą liczbę i typy argumentów, takie jak: `Geometry.Translate`, `Mesh.Translate` i `FamilyInstance.Translate`. Ze względu na fakt, że węzły zostały skompilowane jako metody wystąpienia, przekazanie `FamilyInstance` do węzła `Geometry.Translate` nadal zadziała, ponieważ w czasie wykonywania wybrana zostanie metoda wystąpienia `Translate` dla `FamilyInstance`. Było to oczywiście mylące dla użytkowników, ponieważ węzeł nie robił tego, czego oczekiwano na podstawie jego nazwy.

__B. Drugim problemem było to, że metody wystąpienia nie działały z szykami (tablicami) heterogenicznymi__

W czasie wykonywania silnik wykonywania musi ustalić, do której funkcji należy wysłać komunikat. Jeśli dane wejściowe są listą, powiedzmy `list.Translate()`, ponieważ przechodzenie przez każdy element na liście i wyszukiwanie metody dla jego typu jest kosztowne, logika rozpoznawania metod po prostu zakłada, że typ docelowy jest taki sam jak typ pierwszego elementu, i próbuje wyszukać metodę `Translate()` zdefiniowaną dla tego typu. W wyniku tego jeśli typ pierwszego elementu nie jest zgodny z typem docelowym metody (lub nawet jeśli jest `null` lub jest pustą listą), operacja dla całej listy zakończy się niepowodzeniem, nawet gdy na liście znajdują się inne typy, które są zgodne. 

Na przykład jeśli do `Arc.CenterPoint` przekazano dane wejściowe w postaci listy z następującymi typami `[Arc, Line]`, wynik będzie zawierał punkt środkowy dla łuku i wartość `null` dla linii, czyli zgodnie z oczekiwaniami. Jednak w przypadku odwrócenia kolejności cały wynik będzie miał wartość null, ponieważ pierwszy element spowodował niepowodzenie rozpoznawania metody:
### W dodatku Dynamo 1.x: testowany jest tylko pierwszy element listy danych wejściowych w celu rozpoznania metody
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
W wersji 2.0 oba te problemy zostały rozwiązane przez kompilowanie węzłów interfejsu użytkownika do postaci właściwości statycznych i metod statycznych. 

W przypadku metod statycznych rozpoznawanie metody środowiska wykonawczego jest prostsze, a wszystkie elementy na liście wejściowej są iterowane. Na przykład:

Semantyka metody `foo.Bar()` (metody wystąpienia) musi sprawdzić typ `foo` i to, czy jest listą, czy nie, a następnie dopasować ten element do funkcji kandydujących. Jest to kosztowne. Natomiast semantyka metody `Foo.Bar(foo)` (metody statycznej) musi sprawdzić tylko jedną funkcję o typie parametru `foo`.

Oto co dzieje się w wersji 2.0:
* Węzeł właściwości interfejsu użytkownika jest kompilowany do postaci statycznej metody pobierającej: silnik generuje wersję statyczną metody pobierającej dla każdej właściwości. Na przykład węzeł `Point.X` jest kompilowany do postaci statycznej metody pobierającej `Point.get_X(pt)`. Należy pamiętać, że statyczna metoda pobierająca może zostać również wywołana przy użyciu jej aliasu: `Point.X(pt)` w węźle bloku kodu.
* Węzeł metody interfejsu użytkownika jest kompilowany do wersji statycznej: silnik generuje odpowiednią metodę statyczną dla węzła. Na przykład węzeł `Curve.PointAtParameter` jest kompilowany do postaci `Curve.PointAtParameter(curve: Curve, parameter:double)` zamiast do postaci `curve.PointAtParameter(parameter)`. 

**Uwaga:** ta zmiana nie usunęła obsługi metod wystąpień, więc istniejące metody wystąpień używane w węzłach bloku kodu, takie jak `pt.X` i `curve.PointAtParameter(parameter)` w powyższych przykładach, będą nadal działać.

Ten przykład działał wcześniej w wersji 1.x, ponieważ wykres był kompilowany do postaci `point.X;` i właściwość `X` była znajdowana w obiekcie punktu. Teraz jego skompilowany kod nie działa on w wersji 2.0 — metoda `Vector.X(point)` oczekuje wyłącznie typu `Vector`:

![](../images/8-4/1/lang2_5.png)

### Zalety:
**Spójność i przejrzystość:** metody statyczne eliminują wszelkie niejasności dotyczące tego, która metoda będzie wykonywana w czasie wykonywania. Metoda zawsze jest zgodna z węzłem interfejsu użytkownika używanym na wykresie, którego wywołania użytkownik oczekuje.

**Zgodność:** korelacja między kodem a programem wizualnym jest lepsza.

**Instrukcje:** przekazywanie heterogenicznych list wejściowych do węzłów powoduje teraz uzyskanie wartości innych niż null dla typów akceptowanych przez węzeł i wartości null dla typów, które nie implementują tego węzła. Wyniki są bardziej przewidywalne i zapewniają lepsze wskazanie dotyczące dozwolonych typów dla węzła.

### Zastrzeżenie: nierozstrzygnięte niejasne sytuacje z przeciążonymi metodami

Ponieważ dodatek Dynamo ogólnie obsługuje przeciążenia funkcji, mogą w nim wystąpić niejasności, jeśli istnieje inna przeciążana funkcja o tej samej liczbie parametrów. Na przykład jeśli na poniższym wykresie połączymy wartość liczbową z pozycją danych wejściowych `direction` węzła `Curve.Extrude`, a wektor z pozycją danych wejściowych `distance` węzła `Curve.Extrude`, oba węzły będą nadal działać, co jest nieoczekiwane. W takim przypadku mimo że węzły są kompilowane do metod statycznych, silnik nadal nie jest w stanie dostrzec różnicy w czasie wykonywania i wybiera jedną z nich w zależności od typu danych wejściowych. ![](../images/8-4/1/lang2_6.png)
 
### Rozwiązane problemy:
Przejście na semantykę metod statycznych ma następujące skutki uboczne, o których warto tu wspomnieć jako o powiązanych zmianach dotyczących języka w wersji 2.0.

**1\. Utrata zachowania polimorficznego:**

Rozważmy przykład z węzłów `TSpline` w `ProtoGeometry` (uwaga: typ `TSplineTopology` dziedziczy po typie podstawowym `Topology`): węzeł `Topology.Edges`, który był wcześniej kompilowany do metody wystąpienia `object.Edges`, jest teraz kompilowany do metody statycznej `Topology.Edges(object)`. Poprzednie wywołanie zostałoby polimorficznie rozpoznane jako metoda klasy pochodnej `TsplineTopology.Edges` po wybraniu metody na podstawie typu obiektu środowiska wykonawczego. 

![](../images/8-4/1/lang2_7.png)

Natomiast nowe zachowanie statyczne obejmowało wymuszanie wywołania metody klasy bazowej `Topology.Edges`. W rezultacie ten węzeł zwracał obiekty klasy bazowej `Edge` zamiast obiektów klasy pochodnej typu `TSplineEdge`.
 
![](../images/8-4/1/lang2_8.png)

Była to regresja, ponieważ węzły `TSpline` na dalszym etapie programu oczekujące obiektów `TSplineEdges` zwracały niepowodzenia. 

Ten problem został rozwiązany przez dodanie sprawdzania w środowisku wykonawczym w logice wybierania metod w celu sprawdzenia typu wystąpienia względem typu lub podtypu pierwszego parametru metody. W przypadku listy wejściowej uprościliśmy wybieranie metody przez proste sprawdzanie typu pierwszego elementu. Tak więc ostateczne rozwiązanie było kompromisem między częściowo statycznym, a częściowo dynamicznym wyszukiwaniem metod.

**Nowe zachowanie polimorficzne w wersji 2.0:**

![](../images/8-4/1/lang2_9.png)

W takim przypadku ponieważ pierwszy element `a` to `TSpline`, w czasie wykonywania wywoływana jest metoda pochodna `TSplineTopology.Edges`. W rezultacie zwracana jest wartość `null` dla `b` — typu bazowego `Topology`. 

W drugim przypadku ponieważ typ ogólny `Topology` `b` jest pierwszym elementem, wywoływana jest metoda bazowa `Topology.Edges`. Ponieważ `Topology.Edges` akceptuje jako dane wejściowe również typ pochodny `TSplineTopology`, `a`, zwraca `Edges` zarówno dla pozycji danych wejściowych, `a`, jak i dla pozycji danych wejściowych `b`.

![](../images/8-4/1/lang2_10.png)
 
**2\. Regresje wynikające z tworzenia nadmiarowych list zewnętrznych**

Istnieje jedna główna różnica między metodami wystąpień a metodami statycznymi, jeśli chodzi o zachowanie prowadnicy replikacji. W przypadku metod wystąpień poziom danych wejściowych o pojedynczej wartości z prowadnicami replikacji nie jest podwyższany do list, a w przypadku metod statycznych — jest podwyższany.

Rozważmy przykład węzła `Surface.PointAtParameter` ze skratowaniem krzyżowym i pojedynczą powierzchnią wejściową oraz szykami (tablicami) wartości parametrów `u` i `v`. Metoda wystąpienia jest kompilowana do postaci:
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
Daje to w wyniku szyk (tablicę) punktów 2D.
 
Metoda statyczna jest kompilowana do postaci:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
Powoduje to utworzenie listy 3D punktów z nadmiarową najbardziej zewnętrzną listą.

Ten efekt uboczny kompilowania węzłów interfejsu użytkownika do postaci metod statycznych może potencjalnie powodować regresje w takich istniejących przypadkach użycia. Ten problem rozwiązano przez wyłączenie podwyższania poziomu pozycji danych wejściowych o pojedynczych wartościach do listy w przypadku używania ich z prowadnicami replikacji/skratowaniem (patrz następny element).
 
**4\. Wyłączono podwyższanie poziomu do listy w przypadku prowadnic replikacji/skratowania**

W wersji 1.x występowały dwa przypadki, w których poziom pojedynczych wartości był podwyższany do list:

* Gdy dane wejściowe o niższej randze były przekazywane do funkcji oczekujących danych wejściowych o wyższej randze
* Gdy dane wejściowe o niższej randze były przekazywane do funkcji oczekujących tej samej rangi, ale argumenty wejściowe były opatrzone prowadnicami replikacji lub używały skratowania

W wersji 2.0 nie obsługujemy już tego drugiego przypadku, uniemożliwiając podwyższenie poziomu do listy w takich scenariuszach.

Na poniższym wykresie w wersji 1.x jeden poziom prowadnicy replikacji dla każdego z elementów `y` i `z` wymuszał podwyższenie poziomu szyku (tablicy) rangi 1 dla każdego z tych elementów, dlatego wynik miał rangę 3 (po 1 dla każdego elementu `x`, `y` i `z`). Zamiast tego użytkownik oczekiwałby, że wynik będzie miał rangę 1, ponieważ nie jest całkiem oczywiste, że obecność prowadnic replikacji dla pozycji danych wejściowych z pojedynczymi wartościami spowoduje dodanie poziomów do wyniku.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dodatek Dynamo w wersji 1.x: lista punktów 3D

![](../images/8-4/1/lang2_11.png)

W wersji 2.0 obecność prowadnic replikacji dla każdego z argumentów o pojedynczej wartości `y` i `z` nie powoduje podwyższenia poziomu, więc wynikiem jest lista o takim samym wymiarze co wejściowa lista 1D dla `x`. 

### Dodatek Dynamo w wersji 2.0: lista punktów 1D

![](../images/8-4/1/lang2_12.png)

Również problem wspomnianej powyżej regresji spowodowanej kompilacją metod statycznych z generowaniem nadmiarowych list zewnętrznych został rozwiązany przez tę zmianę dotyczącą języka.

Kontynuując przykład przedstawiony powyżej, można zaobserwować, że wywołanie metody statycznej, takie jak:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
powoduje utworzenie listy punktów 3D w dodatku Dynamo 1.x. Jest tak z powodu podwyższenia poziomu pierwszej powierzchni argumentu o pojedynczej wartości do listy, gdy jest używana z prowadnicą replikacji.
 
### Dodatek Dynamo 1.x: podwyższanie poziomu argumentu z prowadnicą replikacji do listy

![](../images/8-4/1/lang2_13.png)

W wersji 2.0 wyłączyliśmy podwyższanie poziomu argumentów o pojedynczych wartościach do list, gdy są używane z prowadnicami replikacji lub ze skratowaniem. W związku z tym teraz wywołanie:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
zwraca po prostu listę 2D, ponieważ poziom powierzchni nie jest podwyższany.

### Dodatek Dynamo 2.0.x: wyłączono podwyższanie poziomu argumentu o pojedynczej wartości z prowadnicą replikacji do listy

![](../images/8-4/1/lang2_14.png)

Ta zmiana usuwa teraz dodawanie nadmiarowego poziomu listy, a także rozwiązuje problem regresji spowodowanej przejściem na kompilację do postaci metody statycznej.

### Zalety:

**Czytelność: ** wyniki są zgodne z oczekiwaniami użytkownika i łatwiejsze do zrozumienia

**Zgodność:** węzły interfejsu użytkownika (z opcją skratowania) i węzły bloku kodu korzystające z prowadnic replikacji zapewniają zgodne wyniki

**Spójność:** 
* Metody wystąpień i metody statyczne są spójne (rozwiązano problemy z semantyką metod statycznych)
* Węzły z pozycjami danych wejściowych i z argumentami domyślnymi zachowują się spójnie (patrz poniżej)

![](../images/8-4/1/lang2_15.png)

## 5\. Zmienne są niemodyfikowalne w węzłach bloku kodu, aby zapobiec aktualizacji asocjacyjnej 

Język DesignScript historycznie obsługiwał dwa paradygmaty programowania — programowanie asocjacyjne i imperatywne. Kod asocjacyjny tworzy wykres zależności na podstawie instrukcji programu, w których zmienne są od siebie zależne. Zaktualizowanie zmiennej może spowodować aktualizację wszystkich innych zmiennych, które od niej zależą. Oznacza to, że kolejność wykonywania instrukcji w bloku asocjacyjnym nie opiera się na ich kolejności, ale na relacjach zależności między zmiennymi.

W poniższym przykładzie sekwencja wykonywania kodu to wiersze 1 -> 2 -> 3 -> 2. Ponieważ zmienna `b` jest zależna od zmiennej `a`, gdy zmienna `a` zostaje zaktualizowana w wierszu 3, proces wykonywania ponownie przeskakuje do wiersza 2, aby zaktualizować zmienną `b` z uwzględnieniem nowej wartości zmiennej `a`. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
Natomiast jeśli ten sam kod jest wykonywany w kontekście imperatywnym, instrukcje są wykonywane w ramach liniowego, odgórnego przepływu. Imperatywne węzły bloku kodu są zatem odpowiednie do sekwencyjnego wykonywania konstrukcji kodu, takich jak pętle i warunki if-else.

### Niejasności aktualizacji asocjacyjnej:

**1\. Zmienne z zależnością cykliczną:**

Niekiedy zależność cykliczna między zmiennymi nie jest tak oczywista, jak w następującym przypadku. W takich przypadkach, gdy kompilator nie może wykryć cyklu statycznie, może to doprowadzić do nieokreślonego cyklu trybu wykonywania.
```
a = 1;
b = a;
a = b;
```
**2\. Zmienne zależne od samych siebie:**

Jeśli zmienna zależy od samej siebie, to czy jej wartość powinna się kumulować, czy też powinna być resetowana do wartości pierwotnej przy każdej aktualizacji?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
W tym przykładzie geometrii: skoro zmienna `b` sześcianu zależy od samej siebie oraz od walca `a`, to czy przesunięcie suwaka powinno spowodować przesunięcie otworu wzdłuż bloku, czy też wywoływać kumulowanie się otworów wzdłuż jego ścieżki przy każdej aktualizacji pozycji suwaka?

![](../images/8-4/1/lang2_16.gif)

**3\. Aktualizowanie właściwości zmiennych:**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Aktualizowanie funkcji:**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
Z naszego doświadczenia wynika, że aktualizacja asocjacyjna nie okazuje się przydatna w węzłach bloku kodu w kontekście wykresu z przepływem danych opartym na węzłach. Zanim pojawiły się środowiska programowania wizualnego, jedynym sposobem na zbadanie różnych opcji była jawna zmiana wartości niektórych zmiennych w programie. Program tekstowy ma pełną historię aktualizacji zmiennej, podczas gdy w środowisku programowania wizualnego wyświetlana jest tylko najnowsza wartość zmiennej. 

Jeśli ten mechanizm w ogóle był używany przez niektórych użytkowników, najprawdopodobniej był przez nich używany nieświadomie i powodował więcej szkody niż pożytku. Dlatego w wersji 2.0 zdecydowaliśmy się ukryć asocjacyjność w kontekście używania węzłów bloku kodu, czyniąc zmienne niemodyfikowalnymi i zachowując aktualizację asocjacyjną jako natywną funkcję tylko silnika języka DS. Jest to kolejna zmiana wprowadzona z myślą o uproszczeniu obsługi skryptów dla użytkowników.

**Aktualizacja asocjacyjna została wyłączona w węzłach bloku kodu poprzez uniemożliwienie ponownego definiowania zmiennej:** ![](../images/8-4/1/lang2_17.png)

**Indeksowanie listy jest nadal dozwolone w węzłach bloku kodu**

Wprowadzono wyjątek dotyczący indeksowania list — jest to nadal dozwolone w wersji 2.0 z przypisaniem operatora indeksu.

W następnym przykładzie widać, że lista `a` zostaje zainicjowana, ale może być później nadpisana za pomocą przypisania operatora indeksu, a wszelkie zmienne zależne od listy `a` zostaną zaktualizowane w sposób asocjacyjny, jak wskazuje wartość `c`. Ponadto w podglądzie węzła wyświetlane są wartości listy `a` zaktualizowane po ponownym zdefiniowaniu jednego lub większej liczby elementów.

![](../images/8-4/1/lang2_18.png)

## 6\. Zmienne w blokach imperatywnych są zmiennymi lokalnymi w zakresie bloku imperatywnego

Wprowadziliśmy w wersji 2.0 zmiany reguł określania zakresu imperatywnego w celu uniemożliwienia skomplikowanych scenariuszy aktualizacji międzyjęzykowej.

W dodatku Dynamo 1.x sekwencja wykonywania następującego skryptu przebiega wierszami w ten sposób 1 -> 2 -> 4 -> 6 -> 4 — zmiana jest propagowana od zewnętrznych do wewnętrznych zakresów języka. Ponieważ zmienna `y` jest aktualizowana w zewnętrznym bloku asocjacyjnym, a zmienna `x` w bloku imperatywnym jest zależna od zmiennej `y`, sterowanie przesuwa się od zewnętrznego programu asocjacyjnego do języka imperatywnego w wierszu 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

Sekwencja wykonywania w tym następnym przykładzie to wierszami: 1 -> 2 -> 4 -> 2 — zmiana jest propagowana od wewnętrznego zakresu języka do zewnętrznego.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Powyższe scenariusze odnoszą się do aktualizacji międzyjęzykowej, która — podobnie jak aktualizacja asocjacyjna — nie jest zbyt przydatna w węzłach bloku kodu. Aby uniemożliwić złożone scenariusze aktualizacji międzyjęzykowej, zmienne w zakresie imperatywnym ustawiono jako lokalne. 

W poniższym przykładzie w dodatku Dynamo 2.0:
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* Zmienna `x` zdefiniowana w bloku imperatywnym jest teraz lokalna względem zakresu imperatywnego
* Wartościami zmiennych `x` i `y` w zakresie zewnętrznym pozostają odpowiednio `1` i `2`

Każda zmienna lokalna wewnątrz bloku imperatywnego musi zostać zwrócona, jeśli dostęp do jej wartości jest uzyskiwany w zakresie zewnętrznym.

Użytkownik powinien rozważyć następujący przykład:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* Zmienna `y` jest kopiowana lokalnie w zakresie imperatywnym  
* Wartość zmiennej `x` lokalnej w zakresie imperatywnym to `4`
* Zaktualizowanie wartości zmiennej `y` w zakresie zewnętrznym nadal powoduje zaktualizowanie zmiennej `x` z powodu aktualizacji międzyjęzykowej, ale jest wyłączone w blokach kodu w wersji 2.0 z powodu niemodyfikowalności zmiennych
* Wartościami zmiennych `x` i `y` w zewnętrznym zakresie asocjacyjnym pozostają odpowiednio `1` i `2`

## 7\. Listy i słowniki

W dodatku Dynamo 1.x listy i słowniki były reprezentowane przez pojedynczy, ujednolicony kontener, który mógł być indeksowany zarówno za pomocą indeksu będącego liczbą całkowitą, jak i klucza innego niż liczba całkowita. Poniższa tabela zawiera podsumowanie oddzielenia list i słowników w wersji 2.0 oraz reguły nowego typu danych Dictionary, czyli słowników:

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Inicjowanie listy**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Pusta lista**                | `a = {};`                   | `a = [];`                                |
| **Inicjowanie słownika** | **Można dynamicznie dołączać pozycje do tego samego słownika:** | **Można tylko tworzyć nowe słowniki:** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Tworzy pusty słownik |
|                           | `a[“baz”] = 3;`             |                                          |
| **Indeksowanie słownika**   | **Indeksowanie za pomocą kluczy**            | **Składnia indeksowania pozostaje taka sama**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Klucze słownika**       | **Wszystkie typy kluczy były dozwolone**  | **Dozwolone są tylko klucze w postaci ciągów**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Nowa składnia listy `[]`
W wersji 2.0 składnia inicjowania listy została zmieniona z nawiasów klamrowych `{}` na nawiasy kwadratowe `[]`. Wszystkie skrypty w wersji 1.x są automatycznie migrowane do nowej składni po ich otwarciu w wersji 2.0. 

**Uwaga dotycząca domyślnych atrybutów argumentów w węzłach Zero Touch:**

Należy jednak pamiętać, że automatyczna migracja nie zadziała w przypadku starej składni używanej w domyślnych atrybutach argumentów. W razie potrzeby autorzy muszą ręcznie zaktualizować definicje metod Zero Touch, aby używać nowej składni w domyślnych atrybutach argumentów `DefaultArgumentAttribute`.

**Uwaga dotycząca indeksowania:**

Nowe zachowanie indeksowania uległo zmianie w niektórych przypadkach. Indeksowanie listy/słownika za pomocą dowolnej listy indeksów/kluczy przy użyciu operatora `[]` powoduje teraz zachowanie struktury listy wejściowej indeksów/kluczy. Poprzednio zawsze powodowało to zwrócenie listy wartości 1D:
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Składnia inicjowania słownika:

Składni nawiasów klamrowych `{}` można używać do inicjowania słownika tylko w 
```
dict = {<key> : <value>, …}; 
```
formacie par klucz-wartość, w którym jako kluczy `<key>` można używać tylko ciągów, a poszczególne pary klucz-wartość są oddzielone przecinkami.

![](../images/8-4/1/lang2_19.png)

Metoda Zero Touch `Dictionary.ByKeysValues` może być używana jako bardziej wszechstronny sposób inicjowania słownika poprzez przekazanie odpowiednio listy kluczy i listy wartości oraz stosowanie wszelkich rozszerzeń związanych z używaniem metod Zero Touch, takich jak prowadnice replikacji itp.

![](../images/8-4/1/lang2_20.png)

### Dlaczego nie używamy wyrażeń dowolnych w składni inicjowania słownika?

Eksperymentowaliśmy z pomysłem używania wyrażeń dowolnych dla kluczy w składni inicjowania par klucz-wartość słownika i stwierdziliśmy, że może to prowadzić do mylących wyników, zwłaszcza gdy składnia taka jak `{keys : vals}` (gdzie `keys` i `vals` to listy) zakłócała działanie innych funkcji języka DesignScript, takich jak replikacja, i dawała inne wyniki z poziomu węzła inicjatora Zero Touch. 

Na przykład mogą istnieć inne przypadki, takie jak ta instrukcja, w których trudno byłoby zdefiniować oczekiwane zachowanie:
```
dict = {["foo", "bar"] : "baz" };
```
Dalsze dodawanie do tego składni prowadnicy replikacji itp., a nie samych tylko identyfikatorów, byłoby sprzeczne z ideą upraszczania języka. 

_Moglibyśmy_ w przyszłości rozszerzyć obsługę kluczy słowników tak, aby można było stosować wyrażenia dowolne, ale musielibyśmy również zadbać o to, aby interakcja z innymi funkcjami języka była spójna i zrozumiała kosztem zwiększenia złożoności, zamiast uczynić system nieco mniej zaawansowanym, ale za to łatwiejszym do zrozumienia. Przy tym zawsze dostępny jest alternatywny sposób obsługi tej operacji za pomocą metody `Dictionary.ByKeysValues(keyList, valueList)`, która oferuje mechanizm podobny do pominiętego.

### Interakcja z węzłami Zero Touch:

__1\. Węzeł Zero Touch zwracający słownik .NET jest zwracany jako słownik dodatku Dynamo__

**Rozważmy następującą metodę języka C# Zero Touch zwracającą obiekt IDictionary:** ![](../images/8-4/1/lang2_21.png)

**Odpowiednia wartość zwracana przez węzeł ZT jest organizowana jako słownik dodatku Dynamo:** ![](../images/8-4/1/lang2_22.png)

__2\. Węzły z wieloma pozycjami zwracanymi są wyświetlane w podglądzie jako słowniki__

**Węzeł Zero Touch zwracający obiekt IDictionary z atrybutem określającym wiele pozycji zwracanych zwraca słownik dodatku Dynamo:** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. Słownik dodatku Dynamo można przekazać jako dane wejściowe do węzła Zero Touch akceptującego słownik .NET__

**Metoda ZT z parametrem IDictionary:** ![](../images/8-4/1/lang2_25.png)

**Węzeł ZT akceptuje słownik dodatku Dynamo jako dane wejściowe:** ![](../images/8-4/1/lang2_26.png)

### Podgląd słownika w węzłach z wieloma pozycjami zwracanymi

Słowniki to nieuporządkowane pary klucz-wartość. Zgodnie z tą ideą nie ma więc gwarancji, że podglądy par klucz-wartość węzłów zwracających słowniki zostaną uporządkowane w kolejności zgodnej z kolejnością wartości zwracanych tych węzłów. 

Zrobiliśmy jednak wyjątek dla węzłów z wieloma pozycjami zwracanymi, które mają zdefiniowany atrybut `MultiReturnAttribute`. W poniższym przykładzie węzeł `DateTime.Components` jest węzłem z wieloma pozycjami zwracanymi i podgląd tego węzła odzwierciedla jego pary klucz-wartość w kolejności zgodnej z kolejnością portów wyjściowych w węźle — jest to również kolejność, w jakiej pozycje danych wyjściowych są określone na podstawie atrybutu `MultiReturnAttribute` w definicji węzła.

Należy również pamiętać, że podglądy bloków kodu nie są uporządkowane, w przeciwieństwie do węzła interfejsu użytkownika, ponieważ dla węzła bloku kodu nie istnieją informacje dotyczące portów danych wyjściowych (w postaci atrybutu określającego wiele pozycji zwracanych): ![](../images/8-4/1/lang2_27.png)
