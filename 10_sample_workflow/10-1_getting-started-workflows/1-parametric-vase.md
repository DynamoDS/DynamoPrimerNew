---
description: suggested exercise
---

# Wazon parametryczny

Utworzenie wazonu parametrycznego to doskonały sposób na rozpoczęcie nauki korzystania z dodatku Dynamo.

Ten proces roboczy ilustruje:

* Sterowanie zmiennymi w projekcie za pomocą suwaków liczb.
* Tworzenie i modyfikowanie elementów geometrycznych za pomocą węzłów.
* Wizualizowanie wyników projektu w czasie rzeczywistym.

![](../../1\_introduction/images/1-2/vase1.gif)

## Definiowanie celów

Zanim przejdziemy do dodatku Dynamo, zaprojektujmy wazon koncepcyjnie.

Załóżmy, że zaprojektujemy wazon gliniany z uwzględnieniem praktyk wytwarzania stosowanych przez garncarzy. Garncarze zwykle używają koła garncarskiego do produkcji wazonów walcowych. Naciskając na różnych wysokościach wazonu, mogą zmienić jego kształt i tworzyć różne wzory.

Do zdefiniowania wazonu użyjemy podobnej metodologii. Utworzymy 4 okręgi na różnych wysokościach i o różnych promieniach, a następnie utworzymy powierzchnię przez wyciągnięcie tych okręgów.

![](../images/10-1/1/vase2.png)

## Pierwsze kroki

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/10-1/1/DynamoSampleWorkflow-vase.dyn" %}

Potrzebne są węzły reprezentujące sekwencję operacji wykonywanych przez dodatek Dynamo. Ponieważ wiemy, że chcemy utworzyć okrąg, zacznijmy od zlokalizowania węzła, który do tego służy. Użyj **pola wyszukiwania** lub przejdź do **biblioteki**, aby znaleźć węzeł **Circle.ByCenterPointRadius**, i dodaj go do obszaru roboczego

![](../images/10-1/1/vase8.png)

> 1. Wyszukaj > „Circle...”
> 2. Wybierz > „ByCenterPointRadius”
> 3. Węzeł pojawi się w obszarze roboczym

Przyjrzyjmy się bliżej temu węzłowi. Po lewej stronie znajdują się dane wejściowe węzła (_centerPoint_ i _radius_), a po prawej stronie znajdują się dane wyjściowe węzła (Circle). Zwróć uwagę, że dane wyjściowe mają jasnoniebieską linię. Oznacza to, że dane wejściowe mają wartość domyślną. Aby uzyskać więcej informacji na temat danych wejściowych, ustaw kursor na nazwie odpowiedniego wejścia. Dane wejściowe _radius_ wymagają wprowadzenia liczby o podwójnej precyzji (double) i mają wartość domyślną 1.

![](../images/10-1/1/vase10.png)

Zostawimy wartość domyślną _centerPoint_, ale dodamy suwak liczb, **Number Slider**, aby sterować promieniem. Podobnie jak w przypadku węzła **Circle.ByCenterPointRadius**, użyj biblioteki, aby wyszukać **Number Slider**, i dodaj go do wykresu.

Ten węzeł jest nieco inny niż poprzedni węzeł, ponieważ zawiera suwak. Interfejs umożliwia zmianę wartości wyjściowej suwaka.

![](../images/10-1/1/vase13\(1\).gif)

Suwak można skonfigurować za pomocą przycisku listy rozwijanej po lewej stronie węzła. Ograniczmy suwak do maksymalnej wartości 15.

![](../images/10-1/1/vase11.png)

Umieśćmy go po lewej stronie węzła **Circle.ByCenterPointRadius** i połączmy oba węzły, wybierając wyjście **Number Slider** oraz łącząc je z wejściem Radius.

![](../images/10-1/1/vase12.png)

Zmieńmy również nazwę suwaka Number Slider na „Top Radius”, klikając dwukrotnie nazwę węzła.

![](../images/10-1/1/vase14.png)

## Następne kroki

Kontynuujmy dodawanie węzłów i połączeń do logiki w celu zdefiniowania wazonu.

### Tworzenie okręgów o różnych promieniach

Skopiujmy te węzły 4 razy, aby uzyskać okręgi definiujące powierzchnię. Zmień nazwy suwaków Number Slider, jak pokazano poniżej.

![](<../images/10-1/1/vase4 (1).png>)

> 1. Okręgi są tworzone za pomocą punktu środkowego i promienia

### Przesuwanie okręgów na wysokości wazonu

Brakuje nam kluczowego parametru wazonu: jego wysokości. Aby sterować wysokością wazonu, należy utworzyć kolejny suwak liczb. Dodamy również węzeł bloku kodu: **Code Block**. Bloki kodu ułatwiają dodawanie do procesu roboczego spersonalizowanych fragmentów kodu. Użyjemy bloku kodu do pomnożenia suwaka wysokości przez różne współczynniki, co pozwoli nam rozmieścić okręgi wzdłuż wysokości wazonu.

![](../images/10-1/1/vase15\(1\).png)

Następnie za pomocą węzła **Geometry.Translate** umieścimy okręgi na żądanej wysokości. Ponieważ chcemy rozmieścić okręgi w wazonie, użyjemy bloków kodu do pomnożenia parametru wysokości przez współczynnik.

![](../images/10-1/1/vase5.png)

> 2\. Okręgi są przesuwane (przekształcane) o zmienną na osi Z.

### Tworzenie powierzchni

Aby utworzyć powierzchnię za pomocą węzła **Surface.ByLoft**, należy połączyć wszystkie przekształcone okręgi w listę. Użyjemy węzła **List.Create**, aby połączyć wszystkie okręgi w jedną listę, a następnie wyprowadzimy tę listę do węzła **Surface.ByLoft**, aby wyświetlić wyniki.

Wyłączmy również podgląd w innych węzłach, aby wyświetlić tylko wyświetlanie Surface.ByLoft.

![](<../images/10-1/1/vase6 (1).png>)

> 3\. Przez wyciągnięcie przekształconych okręgów zostanie utworzona powierzchnia.

## Wyniki

Nasz proces roboczy jest gotowy. Teraz możemy użyć węzła **Number Slider** zdefiniowanego w skrypcie, aby utworzyć różne projekty wazonów.

![](../../1\_introduction/images/1-2/vase1.gif)

![](../images/10-1/1/vase7.png)
