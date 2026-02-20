# Aktualizace balíčků a knihoven aplikace Dynamo pro aplikaci Dynamo 4.x

### Úvod <a href="#introduction" id="introduction"></a>

Tato část obsahuje informace o problémech, se kterými se můžete setkat při migraci grafů, balíčků a knihoven do aplikace Dynamo 4.x. Aplikace Dynamo 4.0 přináší:
* Významná vylepšení výkonu
* Aktualizace zaměřené na stabilitu a opravy chyb
* Modernizaci základu kódu
* Odebrání rozhraní API, která byla ve verzi 1.x dříve označena jako zastaralá
* Významnou aktualizaci modulu runtime z erze .NET 8 na .NET 10
* Nastavení PythonNet 3 jako výchozího modulu jazyka Python pro všechny nové uzly jazyka Python

Migrace na rozhraní .NET 10 zajišťuje soulad aplikace Dynamo s technologickým harmonogramem společnosti Microsoft, a to s dostatečným předstihem před ukončením podpory rozhraní .NET 8 v listopadu 2026.

Při spuštění aplikace Dynamo 4.0 budete vyzváni k aktualizaci na rozhraní .NET 10, pokud jste tak ještě neučinili. Autoři balíčků musí aktualizovat své projekty tak, aby cílily na .NET 10, aby byla zajištěna plná kompatibilita.

Všechny nové uzly jazyka Python vytvořené v aplikaci Dynamo 4.0+ jsou ve výchozím nastavení založeny na modulu PythonNet3. Nemusíte se obávat zpětné kompatibility: pokud pracujete v prostředí s více verzemi (například Revit nebo Civil 3D 2025/2026), nainstalujte balíček PythonNet3 Engine v aplikaci Dynamo 3.3–3.6, čímž zajistíte zachování kompatibility. Další informace najdete [zde](https://dynamobim.org/dynamo-core-4-0-release/).

Rozhraní API a uzly, které byly ve verzi 1.x označeny jako zastaralé, byly v aplikaci Dynamo 4.0 odebrány. [Úplný seznam změn naleznete zde](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Kompatibilita balíčku <a href="#package-compatibility" id="package-compatibility"></a>

#### Používání balíčků aplikace Dynamo 2.x a 3.x v aplikaci Dynamo 4.x 
Protože aplikace Dynamo 4.x nyní využívá modul runtime rozhraní .NET 10, není u balíčků, které byly vytvořeny pro aplikaci Dynamo 2.x (*pomocí rozhraní .NET 48*) a Dynamo 3.x (*pomocí rozhraní .NET 8*) , zaručeno, že budou fungovat v aplikaci Dynamo 4.x. Při pokusu o stažení balíčku v aplikaci Dynamo 4.x, který byl publikován z verze aplikace Dynamo nižší než 4.0, se zobrazí upozornění, že balíček pochází ze starší verze aplikace.

**To neznamená, že balíček nebude fungovat.** Jedná se pouze o upozornění, že může dojít k problémům s kompatibilitou, a obecně je vhodné zkontrolovat, zda existuje novější verze, která byla vytvořena speciálně pro aplikaci Dynamo 4.x.

Tento typ upozornění můžete také zaznamenat v souborech protokolu aplikace Dynamo při načítání balíčku. Pokud vše funguje správně, můžete ho ignorovat.

#### Používání balíčků aplikace Dynamo 4.x v aplikaci Dynamo 2.x 

Je velmi nepravděpodobné, že balíček vytvořený pro aplikaci Dynamo 4.x (*pomocí rozhraní .Net 10*) bude fungovat v aplikaci Dynamo 2.x. Pokud se v aplikaci Dynamo 2.x pokusíte nainstalovat balíčky vytvořené pro aplikaci Dynamo 4.x, zobrazí se níže uvedené upozornění.

![Upozornění na kompatibilitu balíčku](images/6-2-packages-new-version-compatibility-warning.png)


#### Používání balíčků aplikace Dynamo 4.x v aplikaci Dynamo 3.x 

Balíček vytvořený pro aplikaci Dynamo 4.x (*pomocí rozhraní .Net 10*) může fungovat v aplikaci Dynamo 3.x, pokud všechna rozhraní API použitá v balíčku existují v rozhraní .NET 8. Funkčnost však není zaručena. Pokud se v aplikaci Dynamo 3.x pokusíte nainstalovat balíčky vytvořené pro aplikaci Dynamo 4.x, zobrazí se níže uvedené upozornění.

![Upozornění na kompatibilitu balíčku](images/6-2-packages-compatibility-warning.png)

#### Doporučené postupy pro autory balíčků 
Doporučeným postupem je upravit soubor .csproj tak, aby projekt cílil současně na rozhraní .NET 8 i .NET 10.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Tento přístup zajistí:
* Podporu verzí aplikace Dynamo hostovaných aplikací Revit, které nadále využívají rozhraní .NET 8.
* Kompatibilitu se samostatnou aplikací Dynamo 4.x využívající rozhraní .NET 10.