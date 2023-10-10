# Biblioteka węzłów

Wcześniej wspomniano, że **węzły** są podstawowymi elementami wykresu Dynamo i są zorganizowane w logiczne grupy w **bibliotece**. W dodatku Dynamo for Civil 3D w bibliotece znajdują się dwie kategorie (czyli **półki**), które zawierają węzły przeznaczone do pracy z obiektami programów AutoCAD i Civil 3D, takimi jak linie trasowania, profile, korytarze, odniesienia do bloków itp. Pozostała część biblioteki zawiera węzły, które są bardziej ogólne i są spójne we wszystkich „odmianach” dodatku Dynamo (takich jak dodatek Dynamo dla programu Revit, Dynamo Sandbox itp.).

{% hint style="info" %}\r\n Aby uzyskać więcej informacji na temat organizacji węzłów w podstawowej bibliotece dodatku Dynamo, skorzystaj z sekcji [2-library.md](../3\_user\_interface/2-library.md "mention"). \r\n{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Biblioteka węzłów w dodatku Dynamo for Civil 3D</p></figcaption></figure>

> 1. Węzły przeznaczone specjalnie do pracy z obiektami programów AutoCAD i Civil 3D
> 2. Węzły ogólnego przeznaczenia
> 3. Węzły z **pakietów** innych producentów, które można zainstalować oddzielnie

{% hint style="warning" %}\r\n W przypadku używania węzłów z półek programów AutoCAD i Civil 3D dany wykres dodatku Dynamo będzie działał tylko w dodatku Dynamo for Civil 3D. Jeśli wykres dodatku Dynamo for Civil 3D zostanie otwarty w innym miejscu (na przykład w dodatku Dynamo dla programu Revit), węzły te zostaną oznaczone ostrzeżeniami i nie będą uruchamiane. \r\n{% endhint %}

{% hint style="info" %}\r\n **Dlaczego istnieją dwie oddzielne półki dla programów AutoCAD i Civil 3D?**

Ta organizacja pozwala odróżnić węzły dla natywnych obiektów programu AutoCAD (takich jak linie, polilinie, odniesienia do bloków itp.) od węzłów dla obiektów programu Civil 3D (jak linie trasowania, korytarze, powierzchnie itp.). Z technicznego punktu widzenia programy AutoCAD i Civil 3D to dwa oddzielne komponenty: program AutoCAD jest aplikacją bazową, a program Civil 3D jest produktem na niej opartym. \r\n{% endhint %}

## Hierarchia węzłów

Podczas pracy z węzłami programów AutoCAD i Civil 3D ważne jest pełne zrozumienie hierarchii obiektów na poszczególnych półkach. Pamiętasz systematykę organizmów z biologii? Królestwo, typ, gromada, rząd, rodzina, rodzaj, gatunek? Obiekty programów AutoCAD i Civil 3D są skategoryzowane w podobny sposób. Przeanalizujmy kilka przykładów, aby to wyjaśnić.

### Obiekty programu Civil

Użyjmy jako przykładu linii trasowania.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Załóżmy, że chcemy zmienić nazwę linii trasowania. W związku z tym następnym dodawanym węzłem będzie węzeł **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

Na początku może to wydawać się nieintuicyjne. Czym jest obiekt programu Civil, **CivilObject**, i dlaczego w bibliotece nie ma węzła **Alignment.SetName**? Odpowiedź jest związana z pojęciami _wielokrotnego wykorzystania_ i _prostoty_. Jeśli się nad tym zastanowić, proces zmiany nazwy obiektu programu Civil 3D jest taki sam niezależnie od tego, czy obiekt jest linią trasowania, korytarzem, profilem, czy czymś innym. Dlatego zamiast powtarzających się węzłów, które zasadniczo działają tak samo (np. **Alignment.SetName, Corridor.SetName, Profile.SetName** itp.), najlepszym rozwiązaniem jest opakowanie tej funkcjonalności w pojedynczy węzeł. Do tego właśnie służy węzeł **CivilObject.SetName**.

Można też spojrzeć na tę kwestię pod kątem _relacji_. Linia trasowania i korytarz to typy **obiektów programu Civil**, podobnie jak jabłko i gruszka to typy owoców. Węzły obiektów programu Civil mają zastosowanie do dowolnego typu obiektu programu Civil, tak jak w przypadku obieraczki, za pomocą której można obrać zarówno jabłko, jak i gruszkę. Gdybyśmy używali osobnej obieraczki to każdego rodzaju owoców, w kuchni panowałby straszny bałagan. W tym sensie biblioteka węzłów Dynamo przypomina kuchnię.

### Obiekty

Pójdźmy o krok dalej. Załóżmy, że chcemy zmienić warstwę linii trasowania. Służy do tego węzeł **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Dlaczego nie ma węzła o nazwie **CivilObject.SetLayer**? Mają tutaj zastosowanie te same zasady wielokrotnego wykorzystania i prostoty, które omówiliśmy wcześniej. Właściwość _warstwa_ jest wspólna dla wszystkich obiektów w programie AutoCAD, które można rysować lub wstawiać, takich jak linia, polilinia, tekst, odniesienie do bloku itp. Obiekty programu Civil 3D, takie jak linie trasowania i korytarze, należą do tej samej kategorii, dlatego każdy węzeł, który ma zastosowanie do **obiektu**, może być też używany z dowolnym **obiektem programu Civil**.

