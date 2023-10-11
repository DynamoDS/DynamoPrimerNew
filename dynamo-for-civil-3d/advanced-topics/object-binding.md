# Wiązanie obiektów

Dodatek Dynamo for Civil 3D zawiera bardzo wydajny mechanizm „zapamiętywania” obiektów tworzonych przez poszczególne węzły. Ten mechanizm jest nazywany **wiązaniem obiektów** i umożliwia wykresowi Dynamo generowanie spójnych wyników przy każdym jego uruchomieniu w tym samym dokumencie. Jest to bardzo pożądane w wielu sytuacjach, ale w pewnych innych sytuacjach użytkownik może chcieć mieć większą kontrolę nad zachowaniem dodatku Dynamo. W tej sekcji opisano działanie wiązania obiektów i sposoby jego wykorzystywania.

## Przykład

Rozważmy ten wykres, który tworzy okrąg w obszarze modelu na bieżącej warstwie.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Prosty wykres do tworzenia okręgu</p></figcaption></figure>

Zwróćmy uwagę, co się dzieje, gdy promień zostanie zmieniony.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Modyfikowanie danych wejściowych promienia w dodatku Dynamo</p></figcaption></figure>

To jest wiązanie obiektów w działaniu. Domyślnym zachowaniem dodatku Dynamo jest _zmodyfikowanie_ promienia okręgu, a nie utworzenie nowego okręgu przy każdej zmianie danych wejściowych promienia. Dzieje się tak, ponieważ węzeł **Object.ByGeometry** „pamięta”, że utworzył ten _konkretny_ okrąg przy każdym uruchomieniu wykresu. Ponadto dodatek Dynamo zapisze te informacje, tak aby po następnym otwarciu dokumentu programu Civil 3D i uruchomieniu wykresu dodatek zachowywał się dokładnie tak samo.

## Inny przykład

Przyjrzyjmy się przykładowi, w którym można zmienić domyślne zachowanie dodatku Dynamo w zakresie wiązania obiektów. Załóżmy, że chcemy utworzyć wykres, w którym w środku okręgu jest umieszczany tekst. Celem tego wykresu jest możliwość wielokrotnego uruchamiania go i za każdym razem umieszczania nowego tekstu w wybranym okręgu. Oto jak może wyglądać wykres.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Prosty wykres, który umieszcza tekst w środku wybranego okręgu</p></figcaption></figure>

Jednak faktyczne działanie po wybraniu innego okręgu wygląda tak.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Domyślne zachowanie dodatku Dynamo w przypadku wybrania nowego okręgu</p></figcaption></figure>

Wygląda na to, że wraz z każdym uruchomieniem wykresu tekst zostaje usunięty i ponownie utworzony. W rzeczywistości położenie tekstu jest _modyfikowane_ w zależności od wybranego okręgu. To ten sam tekst — tylko w innym miejscu. Aby za każdym razem tworzyć nowy tekst, należy zmodyfikować ustawienia wiązania obiektów dodatku Dynamo, tak aby nie były zachowywane żadne dane powiązań (patrz [\#binding-settings](object-binding.md#binding-settings "mention") poniżej).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Ustawienia wiązania obiektów</p></figcaption></figure>

Po wprowadzeniu tej zmiany otrzymujemy zachowanie, o które chodziło.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Zachowanie z wyłączonym wiązaniem obiektów</p></figcaption></figure>

## Ustawienia wiązań

Dodatek Dynamo for Civil 3D umożliwia modyfikowanie domyślnego zachowania wiązania obiektów za pomocą ustawień **Przechowywanie danych powiązania** w menu dodatku **Dynamo**.

{% hint style="info" %}
 Należy pamiętać, że opcje przechowywania danych powiązania są dostępne w programie **Civil 3D 2022.1** i w nowszych wersjach. 
{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Wszystkie opcje są domyślnie włączone. Poniżej podsumowano funkcje poszczególnych opcji.

### Opcja 1\. Nie zachowano danych powiązań

Gdy ta opcja jest włączona, dodatek Dynamo „zapomina” o obiektach, które utworzył podczas ostatniego uruchomienia wykresu. Wykres może być więc uruchamiany na dowolnym rysunku w dowolnej sytuacji i za każdym razem utworzy nowe obiekty.

{% hint style="info" %}
 **Zastosowanie**

Używaj tej opcji, aby dodatek Dynamo „zapominał” o tym, co robił w poprzednich uruchomieniach, i za każdym razem tworzył nowe obiekty. 
{% endhint %}

### Opcja 2\. Przechowuj w wykresie dla dodatku Dynamo

Ta opcja oznacza, że metadane wiązania obiektów są serializowane do wykresu (pliku .dyn) podczas jego zapisywania. Jeśli wykres zostanie zamknięty/ponownie otwarty i uruchomiony na **tym samym rysunku**, wszystko powinno działać tak samo, jak działało ostatnio. Jeśli wykres zostanie uruchomiony na **innym rysunku**, dane powiązania zostaną usunięte z wykresu i zostaną utworzone nowe obiekty. Oznacza to, że po otwarciu oryginalnego rysunku i ponownym uruchomieniu wykresu oprócz starych obiektów zostaną utworzone nowe obiekty.

{% hint style="info" %}
 **Zastosowanie**

Używaj tej opcji, aby dodatek Dynamo „pamiętał” obiekty, które utworzył podczas ostatniego uruchomienia na **określonym rysunku**. 
{% endhint %}

{% hint style="warning" %}
 Ta opcja jest najodpowiedniejsza w sytuacjach, gdy możliwe jest zachowanie relacji 1:1 między **konkretnym rysunkiem** a wykresem Dynamo. W przypadku wykresów przeznaczonych do uruchamiania na wielu rysunkach lepsze są opcje 1 i 3\. 
{% endhint %}

### Opcja 3\. Przechowuj w rysunku dla dodatku Dynamo

Ta opcja działa podobnie do opcji 2, ale w tym przypadku dane wiązania obiektów są serializowane na rysunku, a nie na wykresie (w pliku .dyn). Jeśli wykres zostanie zamknięty/ponownie otwarty i uruchomiony na **tym samym rysunku**, wszystko powinno działać tak samo, jak działało ostatnio. Jeśli wykres zostanie uruchomiony na **innym rysunku**, dane powiązania nadal będą zachowane na oryginalnym rysunku, ponieważ są zapisywane w rysunku, a nie w wykresie.

{% hint style="info" %}
 **Zastosowanie**

Używaj tej opcji, aby móc użyć tego samego wykresu na **wielu rysunkach** i aby dodatek Dynamo „pamiętał”, co robił na każdym z nich. 
{% endhint %}

### Opcja 4\. Przechowuj w rysunku dla Odtwarzacza Dynamo

Po pierwsze: ta opcja nie ma wpływu na interakcje wykresu z rysunkiem w przypadku uruchamiania wykresu za pomocą głównego interfejsu dodatku Dynamo. Ta opcja ma zastosowanie _tylko_ w przypadku, gdy wykres jest uruchamiany za pomocą Odtwarzacza Dynamo.

{% hint style="info" %}
 Jeśli nie znasz jeszcze Odtwarzacza Dynamo Player, skorzystaj z sekcji [dynamo-player.md](../dynamo-player.md "mention"). 
{% endhint %}

Jeśli wykres zostanie uruchomiony za pomocą interfejsu głównego dodatku Dynamo, a następnie zostanie zamknięty i uruchomiony za pomocą Odtwarzacza Dynamo, obok obiektów utworzonych wcześniej zostaną utworzone nowe obiekty. Jednak po jednokrotnym wykonaniu wykresu przez Odtwarzacz Dynamo dane wiązania obiektów zostaną zserializowane do rysunku. Jeśli więc wykres jest uruchamiany wielokrotnie za pomocą Odtwarzacza Dynamo, zamiast tworzenia obiektów, zostają zaktualizowane istniejące obiekty. Jeśli wykres zostanie uruchomiony za pomocą Odtwarzacza Dynamo na **innym rysunku**, dane powiązania nadal będą zachowane na oryginalnym rysunku, ponieważ są zapisywane w rysunku, a nie w wykresie.

{% hint style="info" %}
 **Zastosowanie**

Używaj tej opcji, aby móc uruchamiać wykres za pomocą Odtwarzacza Dynamo na wielu rysunkach i aby dodatek Dynamo „pamiętał”, co robił na każdym z nich 
{% endhint %}
