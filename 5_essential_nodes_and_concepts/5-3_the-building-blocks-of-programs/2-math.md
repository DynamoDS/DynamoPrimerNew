# Matematyka

Jeśli najprostszą formą danych są liczby, najprostszym sposobem powiązywania tych liczb jest matematyka. Od prostych operatorów, takich jak dzielenie czy funkcje trygonometryczne, po bardziej złożone formuły — matematyka stanowi doskonały sposób na rozpoczęcie badania zależności i wzorców między liczbami.

### Operatory arytmetyczne

Operatory to zestaw komponentów, w których używane są funkcje algebraiczne z dwiema wejściowymi wartościami liczbowymi dającymi jedną wartość wyjściową (dodawanie, odejmowanie, mnożenie, dzielenie itp.). Można je znaleźć w obszarze Operatory > Operacje.

| Ikona                                                  | Nazwa (składnia)     | Dane wejściowe                     | Dane wyjściowe      |
| ----------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| ![](<../images/5-1/addition(1)(1) (1) (1).jpg>)       | Dodawanie (**+**)       | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Subtraction(1)(1) (1) (1).jpg>)    | Odejmowanie (**-**)  | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Multiplication(1)(1) (1) (1).jpg>) | Mnożenie (*) | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Division(1)(1) (1) (1).jpg>)       | Dzielenie (**/**)    | var[]...[], var[]...[] | var[]...[] |

## Ćwiczenie: formuła złotej spirali

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### Część I. Formuła parametryczna

Połącz operatory i zmienne, aby utworzyć bardziej złożone zależności za pomocą **formuł**. Użyj suwaków, aby utworzyć formułę, którą można sterować za pomocą parametrów wejściowych.

1\. Utwórz sekwencję liczb reprezentującą „t” w równaniu parametrycznym. Lista powinna być wystarczająco duża, aby można było zdefiniować spiralę.

**Number Sequence**: zdefiniuj sekwencję liczb w oparciu o trzy wejścia: _start, amount_ i _step_.

![](../images/5-3/2/math-partI-01.jpg)

2\. W powyższym kroku utworzono listę liczb definiujących dziedzinę parametryczną. Następnie utwórz grupę węzłów reprezentujących równanie złotej spirali.

Złota spirala jest zdefiniowana jako równanie:

$$
x = r cos θ = a cos θ e^{bθ}
$$

$$
y = r sin θ = a sin θe^{bθ}
$$

Poniższa ilustracja przedstawia złotą spiralę w postaci programowania wizualnego. Podczas przechodzenia przez grupę węzłów należy zwrócić uwagę na analogię pomiędzy programem wizualnym a równaniem pisemnym.

![](../images/5-3/2/math-partI-02.jpg)

> a. **Number Slider**: dodaj dwa suwaki liczb do obszaru rysunku. Suwaki te reprezentują zmienne _a_ i _b_ równania parametrycznego. Te elementy reprezentują stałą, która jest elastyczna, lub parametry, które można dostosować do żądanego wyniku.
>
> b. **Multiplication (*)**: węzeł mnożenia jest reprezentowany przez gwiazdkę. Użyjemy tego wielokrotnie, aby połączyć zmienne mnożenia.
>
> c. **Math.RadiansToDegrees**: wartości „_t_” muszą zostać przekształcone w stopnie w celu ich oszacowania w funkcjach trygonometrycznych. Należy pamiętać, że podczas szacowania tych funkcji dodatek Dynamo domyślnie obsługuje wartości w stopniach.
>
> d. **Math.Pow**: jako funkcja wartości „_t_” i liczby „_e_” tworzy to ciąg Fibonacciego.
>
> e. **Math.Cos i Math.Sin**: te dwie funkcje trygonometryczne będą różnicować współrzędną x i współrzędną y każdego punktu parametrycznego.
>
> f. **Watch**: teraz widzimy, że wynik to dwie listy — współrzędne _x_ i _y_ punktów użytych do wygenerowania spirali.

### Część II. Od formuły do geometrii

Teraz zestaw węzłów z poprzedniego kroku będzie działać poprawnie, ale to sporo pracy. Aby utworzyć wydajniejszy proces roboczy, zapoznaj się z częścią [DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md) w celu definiowania ciągu wyrażeń Dynamo w jednym węźle. W następnej serii kroków przeanalizujemy używanie równania parametrycznego do rysowania spirali Fibonacciego.

**Point.ByCoordinates:** połącz górny węzeł multiplication z wejściem _„x”_, a dolny — z wejściem _„y”_. Na ekranie pojawi się spirala parametryczna punktów.

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints:** połącz węzeł **Point.ByCoordinates** z poprzedniego kroku z węzłem _points_. Węzeł _connectLastToFirst_ możemy pozostawić bez wejścia, ponieważ nie tworzymy krzywej zamkniętej. Spowoduje to utworzenie spirali przechodzącej przez każdy punkt zdefiniowany w poprzednim kroku.

![](../images/5-3/2/math-partII-02.jpg)

Mamy gotową spiralę Fibonacciego. Przekształcimy to jeszcze bardziej w dwóch osobnych ćwiczeniach, tworząc kształty nautilusa i słonecznika. Są to abstrakcje systemów naturalnych, ale zapewni to dobrą reprezentację tych dwóch różnych zastosowań spirali Fibonacciego.

### Część III. Od spirali do nautilusa

**Circle.ByCenterPointRadius:** użyjemy tutaj węzła circle z tymi samymi wejściami co w poprzednim kroku. Domyślną wartością promienia jest _1,0_, dlatego natychmiast widoczne będą wyniki okręgów. Od razu staje się jasne, w jaki sposób te punkty odbiegają dalej od początku.

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence:** jest to oryginalny szyk „_t_”. Przez połączenie tej pozycji z wartością **Circle.ByCenterPointRadius** środki okręgów wciąż odbiegają dalej od początku, ale promień okręgów rośnie, tworząc interesujący wykres okręgów Fibonacciego.

Jeszcze lepiej, jeśli uda Ci się przekształcić go w wykres 3D.

![](../images/5-3/2/math-partIII-02.gif)

### Część IV. Od nautilusa do ulistnienia

Mamy już powłokę nautilusa — przejdźmy do siatek parametrycznych. Użyjemy podstawowego obrotu na spirali Fibonacciego, aby utworzyć siatkę Fibonacciego, a wynik zostanie wymodelowany na wzór [rosnących nasion słonecznika](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/).

Punktem wyjścia będzie ten sam krok co w poprzednim ćwiczeniu: utworzenie szyku spirali punktów za pomocą węzła **Point.ByCoordinates**.

\![](../images/5-3/2/math-part IV-01.jpg)

Następnie wykonaj te minikroki, aby wygenerować serię spiral o różnych obrotach.

![](../images/5-3/2/math-partIV-02.jpg)

> a. **Geometry.Rotate:** dostępnych jest kilka opcji **Geometry.Rotate**. Należy pamiętać, aby wybrać węzeł z wejściami _geometry_, _basePlane_ i _degrees_. Połącz węzeł **Point.ByCoordinates** z wejściem geometry. Kliknij prawym przyciskiem myszy ten węzeł i upewnij się, że skratowanie jest ustawione na Iloczyn wektorowy
>
> <img src="../images/5-3/2/math-partIV-03crossproduct.jpg" alt="" data-size="original">
>
> b. **Plane.XY:** połącz z wejściem _basePlane_. Wykonamy obrót wokół początku, który ma to samo położenie co podstawa spirali.
>
> c. **Number Range:** na potrzeby wartości wejściowej w stopniach utworzymy wiele obrotów. Można to zrobić szybko za pomocą węzła **Number Range**. Połącz to z wejściem _degrees_.
>
> d. **Number:** aby zdefiniować zakres liczb, dodaj trzy węzły number do obszaru rysunku w kolejności pionowej. Od góry do dołu przypisz odpowiednio wartości _0,0, 360,0_ i _120,0_. Sterują one obrotem spirali. Zwróć uwagę na wyniki z wyjścia węzła **Number Range** po połączeniu trzech węzłów number z tym węzłem.

Nasz wynik zaczyna przypominać wir. Dostosuj niektóre parametry węzła **Number Range** i obserwuj, jak zmieniają się wyniki.

Zmień rozmiar kroku (step) węzła **Number Range** z _120,0_ na _36,0_. Zwróć uwagę, że w ten sposób powstaje więcej obrotów, co zapewnia gęstszą siatkę.

![](../images/5-3/2/math-partIV-04.jpg)

Zmień rozmiar kroku (step) węzła **Number Range** z _36,0_ na _3,6_. Daje to teraz dużo gęstszą siatkę, a kierunkowość spirali jest niejasna. W ten sposób utworzyliśmy słonecznik.

![](../images/5-3/2/math-partIV-05.jpg)
