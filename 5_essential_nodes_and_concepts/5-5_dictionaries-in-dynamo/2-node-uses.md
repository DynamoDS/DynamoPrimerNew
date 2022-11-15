# Nós de dicionário

O Dynamo 2.0 expõe uma variedade de nós de dicionário para serem usados. Isso inclui os nós _criar, ação e consulta_.

![](../images/5-5/2/dictionarynodes-nodes.jpg)

#### Criar

1. `Dictionary.ByKeysValues` criará um dicionário com os valores e chaves fornecidos. _(O número de entradas será o da lista de entradas mais curta)_

#### Ação

2. `Dictionary.Components` produzirá os componentes do dicionário de entrada. _(Isso é o inverso do nó criar)_

3. `Dictionary.RemoveKeys` produzirá um novo objeto de dicionário com as chaves de entrada removidas.

4. `Dictionary.SetValueAtKeys` produzirá um novo dicionário com base nas chaves de entrada e nos valores para substituir o valor atual nas chaves correspondentes.

5. `Dictionary.ValueAtKey` retornará o valor na chave de entrada.

#### Contagem

6. `Dictionary.Count` informará quantos pares de valores-chave estão no dicionário.

7. `Dictionary.Keys` retornará quais chaves estão atualmente armazenadas no dicionário.

8. `Dictionary.Values` retornará quais valores estão armazenados no momento no dicionário.

A relação global de dados com dicionários é uma ótima alternativa ao método antigo de trabalho com índices e listas.
