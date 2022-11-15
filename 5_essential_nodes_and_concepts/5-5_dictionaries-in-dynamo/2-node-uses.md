# Uzly slovníku

Aplikace Dynamo 2.0 nabízí několik uzlů pro slovníky. Mezi ně patří uzly pro _tvorbu, akci a dotazování_.

![](../images/5-5/2/dictionarynodes-nodes.jpg)

#### Tvorba

1\. Uzel `Dictionary.ByKeysValues` vytvoří slovník ze zadaných hodnot a klíčů. _(Počet položek bude odpovídat počtu položek nejkratšího seznamu.)_

#### Akce

2\. Uzel `Dictionary.Components` vytvoří komponenty ze vstupního slovníku. _(Operace opačná k vytvoření slovníku.)_

3\. Uzel `Dictionary.RemoveKeys` vytvoří nový objekt slovníku bez vstupních klíčů.

4\. Uzel `Dictionary.SetValueAtKeys` vytvoří nový slovník na základě vstupních klíčů a hodnot, které nahradí aktuální hodnotu u příslušných klíčů.

5\. Uzel `Dictionary.ValueAtKey` vrátí hodnotu na pozici vstupního klíče.

#### Počet

6\. Uzel `Dictionary.Count` vrátí počet dvojic hodnot a klíčů ve slovníku.

7\. Uzel `Dictionary.Keys` vrátí klíče aktuálně uložené ve slovníku.

8\. Uzel `Dictionary.Values` vrátí hodnoty aktuálně uložené ve slovníku.

Spojování dat ve slovnících může být užitečnou alternativou pro starý způsob práce s indexy a seznamy.
