# Miniatury grafów dla strony głównej dodatku Dynamo

Strona główna dodatku Dynamo wyświetla miniatury grafów na szerokiej karcie. Karta ma proporcje 16:9. Strona główna wyśrodkowuje obraz oraz obcina jego górną i dolną część.

Kwadratowa miniatura może wyglądać na kompletną w narzędziu Generative Design, które pokazuje cały kwadrat. Na stronie głównej ten sam obraz może utracić tytuły lub logo.

Dotyczy to każdego grafu (`.dyn`), a nie tylko grafów znajdujących się w pakiecie. Ustaw obraz we właściwościach grafu. Obraz jest przechowywany w pliku grafu.

{% hint style="info" %}
Umieść tytuły, logo i główny obraz na środkowym **bezpiecznym obszarze**. Górną i dolną część kwadratowej miniatury wykorzystuj wyłącznie jako tło.
{% endhint %}

## Szablony miniatur

Pobierz szablon i oprzyj na nim swój projekt. Środkowy ciemniejszy pas jest tym, co widać na stronie głównej. Jaśniejsze pasy u góry i u dołu są na niej obcinane. Generative Design pokazuje cały kwadrat.

> Pobierz szablon SVG, aby edytować go w aplikacji do grafiki wektorowej, albo szablon PNG, aby umieścić grafikę w edytorze obrazów.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Szablon kwadratowej miniatury. Środkowy ciemniejszy pas jest widoczny na stronie głównej. Jaśniejsze pasy u góry i u dołu są na niej obcinane, ale pozostają widoczne w narzędziu Generative Design."><figcaption><p>Środkowy ciemniejszy pas jest widoczny na stronie głównej. Jaśniejsze pasy u góry i u dołu są na niej obcinane.</p></figcaption></figure>

## Tworzenie miniatury

1. Otwórz szablon SVG lub PNG w edytorze obrazów.
2. Umieść grafikę na różowym kwadracie w szablonie.
3. Umieść tytuł, logo i główny obraz wewnątrz środkowego ciemniejszego pasa.
4. Jaśniejszy górny i dolny pas wykorzystaj tylko na potrzeby tła lub dodatkowej grafiki.
5. Ukryj lub usuń różową prowadnicę, a następnie wyeksportuj plik PNG z **kwadratowym** obrazem.

Użyj **kwadratowego** obrazu (1:1). Na stronie głównej środkowy ciemniejszy pas jest zawsze wyświetlany w przyciętej wersji o proporcjach 16:9. Rozmiar piksela nie zmienia obcięcia. Jeśli zaczynasz od pustego pliku, a nie od szablonu, nadaj obrazowi kształt kwadratu. Szablon miniatury ma wymiary 840 × 840 pikseli.

## Dodawanie miniatury do grafu

Strona główna odczytuje miniaturę z pliku grafu (`.dyn`), a nie z formularza publikowania menedżera pakietów.

1. Otwórz graf w dodatku Dynamo.
2. Wybierz kolejno opcje **Plik > Pokaż właściwości grafu > Ogólne**.
3. Dodaj wyeksportowany plik PNG do pola obrazu.
4. Zapisz graf.

Jeśli graf jest dostarczany w pakiecie, dołącz ten plik `.dyn` podczas publikowania. Zobacz [Publikowanie pakietu](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Sprawdzanie wyniku na stronie głównej

1. Otwórz stronę główną dodatku Dynamo.
2. W obszarze Ostatnie znajdź właśnie zapisany graf.
3. Sprawdź, czy tytuł i logo są w pełni widoczne na karcie.

Jeśli tekst jest obcięty, przesuń go do środka, ponownie wyeksportuj obraz i zaktualizuj właściwości grafu. Nie polegaj wyłącznie na narzędziu Generative Design. Generative Design pokazuje cały kwadrat.
