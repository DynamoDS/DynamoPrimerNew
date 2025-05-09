# Rozdíly mezi výpočetní službou Dynamo a počítačovou aplikací Desktop

Na této stránce naleznete informace o rozdílech, které byste měli znát při psaní programů pro aplikaci Dynamo, které se mají spustit v cloudovém kontextu výpočetní služby Dynamo.

## Co je DaaS?

DaaS, Dynamo jako služba, výpočetní služba Dynamo atd. označují totéž: základní modul runtime Dynamo spuštěný v cloudovém kontextu. To znamená, že graf se nespouští ve vašem počítači. Služba DaaS je v současné době přístupná pouze prostřednictvím rozšíření Dynamo Player pro aplikaci Forma, kde mohou uživatelé nahrávat a spravovat soubory `.dyn` vytvořené v počítači, spouštět soubory `.dyn` sdílené kolegy prostřednictvím tohoto rozšíření nebo používat předinstalované rutiny `.dyn` poskytované společností Autodesk jako vzorky.

Vzhledem k tomu, že grafy běží v tomto cloudovém kontextu, a nikoli ve vašem počítači, DaaS v současné době nemůže přímo využívat tradiční hostitelské kontexty Dynamo (Revit, Civil 3D atd.). Pokud chcete ve svém grafu používat typy z těchto programů, budete je muset serializovat (uložit) do grafu pomocí uzlu `Data.Remember` nebo jiných technik serializace v grafu. Jedná se o podobné pracovní postupy, které se používají při psaní grafů pro generativní návrh v aplikaci Revit.

## Jaká verze aplikace Dynamo provádí můj kód?

Tato verze je založena na verzi 3.x a je často aktualizována na základě opensourcové hlavní větve aplikace Dynamo.

## Jaké balíčky/uzly jsou dostupné v této verzi aplikace Dynamo?

* Většina základních uzlů – s některými konkrétními omezeními, která jsou uvedena v následující části.
* Balíček `DynamoFormaBeta` pro interakci s rozhraním API aplikace Forma.
* `VASA` pro voxelizaci / efektivní analýzu.
* `MeshToolKit` pro manipulaci se sítí. Od verze Dynamo 3.4 je dodávána také předinstalovaná sada nástrojů pro sítě.
* `RefineryToolkit` pro užitečné algoritmy, které umožňují využívat test kolizí, vzdálenost pohledu, nejkratší cestu, isovist, atd.

## Na co si dát pozor při psaní grafů pro DaaS?

* Uzly jazyka Python nebudou fungovat. Ty se _aktuálně_ prostě nepodaří spustit.
* Nelze používat vlastní balíčky.
* Hladina uživatelského rozhraní / zobrazení uzlů se u uzlů uživatelského rozhraní nespustí. Nepředpokládáme, že by se jednalo o problém se základními funkcemi, ale je dobré to mít na paměti, pokud narazíte na chybu související s uzlem s vlastním uživatelským rozhraním.
* Funkce pouze pro systém Windows nebudou fungovat. Pokud se například pokusíte použít registr systému Windows nebo WPF, dojde k chybě.
* Rozšíření pohledů nebudou načtena.
* Uzly souborového systému nebudou příliš užitečné. Všechny soubory, na které odkazujete v místním počítači, nebudou při spuštění v DaaS existovat.
* Uzly interoperability pro Excel/DSOffice nebudou fungovat. Uzly Open XML by měly fungovat.
* Síťové požadavky obecně nebudou fungovat, i když můžete volat rozhraní API aplikace Forma.

## Jak si to mám všechno pamatovat? Co když se to změní?

* V budoucnu máme v úmyslu v aplikaci Dynamo pro počítače poskytnout nástroje, které usnadní stejné fungování grafu v obou kontextech.

## Kolik to bude stát?

* U této beta verze není výpočetní čas v současné době účtován.

## Jak začít?

Začněte tím, že se podíváte na [příspěvek na blogu ](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), [sérii videí na YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) nebo ukázky v rozšíření Forma. Ty vás provedou následujícími tématy:

* Získání přístupu k aplikaci Autodesk Forma
* Instalace doplňku DynamoFormaBeta for Dynamo na počítači a rozšíření Dynamo v aplikaci Forma
* Psaní prvního grafu

## Zabezpečení

* Mějte na paměti, že sdílené grafy jsou uloženy v aplikaci Forma.
* Maximální doba spuštění grafu je aktuálně kratší než 30 minut. Tato hodnota se může změnit.
* Požadavky na spuštění jsou omezeny frekvencí, takže pokud provedete mnoho požadavků na výpočet v příliš krátkém časovém období, mohou se zobrazit chyby.
