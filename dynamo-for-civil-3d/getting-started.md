# Pierwsze kroki

Po zapoznaniu się z ogólnymi informacjami przejdźmy do konkretów i zbudujmy pierwszy wykres dodatku Dynamo w programie Civil 3D.

{% hint style="info" %}
 Jest to prosty przykład, za pomocą którego zademonstrujemy podstawowe funkcje dodatku Dynamo. Zaleca się, aby wykonywać te czynności w nowym pustym dokumencie programu Civil 3D. 
{% endhint %} 

## Otwieranie dodatku Dynamo

Najpierw należy otworzyć pusty dokument w programie Civil 3D. Po otwarciu go przejdź do karty **Zarządzaj** na wstążce programu Civil 3D i wyszukaj panel **Programowanie wizualne**.

![](<../.gitbook/assets/image (7).png>)

Kliknij przycisk **Dynamo**. Spowoduje to uruchomienie dodatku Dynamo w osobnym oknie.

{% hint style="info" %}
 **Jaka jest różnica między dodatkiem Dynamo a Odtwarzaczem Dynamo?**

Dodatek Dynamo to narzędzie używane do tworzenia i uruchamiania wykresów. Odtwarzacz Dynamo to prosty mechanizm do uruchamiania wykresów bez konieczności otwierania ich w dodatku Dynamo.

Przejdź do sekcji [dynamo-player.md](dynamo-player.md "mention"), aby wypróbować tę funkcję. 
{% endhint %} 

## Rozpoczynanie nowego wykresu

Po otwarciu dodatku Dynamo zostanie wyświetlony ekran startowy. Kliknij przycisk **Nowy**, aby otworzyć pusty obszar roboczy.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Ekran startowy dodatku Dynamo</p></figcaption></figure>

{% hint style="info" %}
 **Co z przykładami?**

Dodatek Dynamo for Civil 3D zawiera kilka wstępnie utworzonych wykresów, z których można czerpać pomysły na korzystanie z dodatku Dynamo. Zalecamy przyjrzenie się nim w dogodnym momencie, a także zapoznanie się z sekcją [sample-workflows](sample-workflows/ "mention") w tym przewodniku Primer. 
{% endhint %} 

## Dodanie węzłów

Powinien być teraz widoczny pusty obszar roboczy. Przyjrzyjmy się dodatkowi Dynamo w działaniu. Oto nasz cel:

>  :dart: **Utworzenie wykresu Dynamo, który będzie wstawiał tekst do obszaru modelu.**

Dosyć proste, prawda? Ale zanim zaczniemy, musimy omówić kilka podstawowych kwestii.

Podstawowe elementy wykresu Dynamo są nazywane **węzłami**. Węzeł jest jak mała maszyna — przekazujesz do niego dane, a on wykonuje na nich jakąś pracę i zwraca wyniki. Dodatek Dynamo for Civil 3D zawiera **bibliotekę** węzłów, które można ze sobą łączyć za pomocą **przewodów** w celu utworzenia **wykresu** umożliwiającego wykonywanie większej liczby operacji i zapewniającego lepsze wyniki, niż mógłby zwrócić samodzielny węzeł.

{% hint style="info" %}
 **Co jeśli nigdy wcześniej nie zdarzyło mi się korzystać z dodatku Dynamo?**

Część z tych informacji może być dla Ciebie całkiem nowa, ale nie ma powodu do obaw. W tych sekcjach znajdziesz pomoc.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") 
{% endhint %} 

Utwórzmy wykres. Oto lista wszystkich węzłów, których będziemy potrzebować.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

Te węzły można znaleźć, wpisując ich nazwy na pasku wyszukiwania w bibliotece lub klikając prawym przyciskiem myszy w dowolnym miejscu w obszarze rysunku i wyszukując je w tym miejscu.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>Węzły można umieszczać z poziomu biblioteki lub klikając prawym przyciskiem myszy w obszarze rysunku</p></figcaption></figure>

{% hint style="info" %}
 **Skąd wiadomo, których węzłów użyć i gdzie je znaleźć?**

Węzły w bibliotece są pogrupowane w logiczne kategorie w zależności od tego, do czego służą. Bardziej szczegółową prezentację można znaleźć w sekcji [node-library.md](node-library.md "mention"). 
{% endhint %} 

Oto jak powinien wyglądać ostateczny wykres.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>Wykres ostateczny</p></figcaption></figure>

Podsumujmy to, co tutaj zrobiliśmy:

> 1. Wybraliśmy dokument do pracy. W tym przypadku (tak jak w wielu innych przypadkach) chcemy pracować w aktywnym dokumencie w programie Civil 3D.
> 2. Zdefiniowaliśmy blok docelowy, w którym ma zostać utworzony obiekt tekstowy (w tym przypadku obszar modelu — Model Space).
> 3. Użyliśmy węzła _String_, aby określić, na której warstwie powinien zostać umieszczony tekst.
> 4. Utworzyliśmy punkt za pomocą węzła _Point.ByCoordinates_, aby zdefiniować położenie, w którym ma zostać umieszczony tekst.
> 5. Zdefiniowaliśmy współrzędne X i Y punktu wstawienia tekstu za pomocą dwóch węzłów _Number Slider_.
> 6. Użyliśmy innego węzła _String_ do zdefiniowania zawartości obiektu tekstowego (Text).
> 7. Na koniec utworzyliśmy obiekt tekstowy.

Przyjrzyjmy się wynikom tego nowego wykresu.

## Oglądanie wyniku

W programie Civil 3D upewnij się, że wybrana jest karta **Model**. Powinien zostać wyświetlony nowy obiekt tekstowy utworzony przez dodatek Dynamo.

{% hint style="info" %}
 Jeśli nie widzisz tekstu, może być konieczne uruchomienie polecenia ZOOM -> EXTENTS w celu powiększenia do odpowiedniego miejsca. 
{% endhint %} 

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Świetnie! Teraz wprowadzimy pewne aktualizacje w tekście.

Wróć do wykresu Dynamo i zmień kilka wartości wejściowych, takich jak ciąg tekstowy, współrzędne punktu wstawienia itp. Tekst powinien zostać automatycznie zaktualizowany w programie Civil 3D. Zauważ też, że po odłączeniu jednego z portów wejściowych tekst zostanie usunięty. Po ponownym podłączeniu wszystkiego tekst zostanie utworzony ponownie. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>Gotowy wykres w działaniu</p></figcaption></figure>

</div>

{% hint style="info" %}
 **Dlaczego dodatek Dynamo nie wstawia nowego obiektu tekstowego przy każdym uruchomieniu wykresu?**

Domyślnie dodatek Dynamo „zapamiętuje” utworzone przez siebie obiekty. Jeśli zmienisz wartości wejściowe węzłów, obiekty w programie Civil 3D zostaną zaktualizowane, zamiast utworzenia nowych obiektów. Więcej informacji na temat tego zachowania można znaleźć w sekcji [object-binding.md](advanced-topics/object-binding.md "mention"). 
{% endhint %} 

> :tada: Misja wykonana!

## Następne kroki

Przedstawiono tu zaledwie mały przykład tego, co można zrobić za pomocą dodatku Dynamo for Civil 3D.  Czytaj dalej, aby dowiedzieć się więcej!
