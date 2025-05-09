# Interface de linha de comando do Dynamo

`-o, -O, --OpenFilePath` Instrua o Dynamo a abrir um arquivo de comando e execute os comandos contidos no caminho. Essa opção só é suportada quando executada no DynamoSandbox.  

`-c, -C, --CommandFilePath` Instrua o Dynamo a abrir um arquivo de comando e execute os comandos contidos no caminho. Essa opção só é suportada quando executada no DynamoSandbox.  

`-v, -V, --Verbose` Instrua o Dynamo a enviar todas as avaliações que executa para um arquivo XML no caminho especificado.  

`-g, -G, --Geometry` Instrua o Dynamo a gerar a geometria de todas as avaliações para um arquivo JSON nesse caminho.  

`-h, -H, --help` Obtenha ajuda.  

`-i, -I, --Import` Instrua o Dynamo a importar uma montagem como uma biblioteca de nós. Esse argumento deve ser um caminho de arquivo para um único `.dll`. Se você deseja importar vários `.dlls`, liste-os separados por um espaço: `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath` Caminho relativo ou absoluto para um diretório que contém o ASM. Quando fornecido, em vez de procurar o ASM no disco rígido, ele será carregado diretamente desse caminho.  

`-k, -K, --KeepAlive` Modo Keepalive, deixe o processo do Dynamo em execução até que uma extensão carregada o desligue.  

`--HostName` Identifique a variação do Dynamo associada ao hospedeiro.  

`-s, -S, --SessionId` Identifique a ID da sessão de análise do hospedeiro do Dynamo.  

`-p, -P, --ParentId` Identifique a ID pai da análise do hospedeiro do Dynamo.  

`-x, -X, --ConvertFile` Quando usado em combinação com o indicador `-O`, abre um arquivo `.dyn` no caminho especificado e o converte em `.json`. O arquivo terá a extensão `.json` e estará localizado no mesmo diretório que o arquivo original.  

`-n, -N, --NoConsole` Não confie na janela do console para interagir com a CLI no modo Keepalive.  

`-u, -U, --UserData` Especifique a pasta de dados do usuário a ser usada pelo PathResolver com a CLI.  

`--CommonData` Especifique a pasta de dados comuns a ser usada pelo PathResolver com a CLI.  

`--DisableAnalytics` Desativa a análise no Dynamo durante a vida útil do processo.  

`--CERLocation` Especifique a ferramenta de relatório de erros fatais localizada no disco.  

`--ServiceMode` Especifique a inicialização do modo de serviço.  



#### Por quê 
 Você pode querer controlar o Dynamo por meio da linha de comando por vários motivos, como: 
 
 * Automatizar muitas execuções do Dynamo
 * Testar os gráficos do Dynamo (veja também -c quando usar o DynamoSandbox)
 * Executar uma sequência de gráficos do Dynamo em uma ordem específica
 * Escrever arquivos em lote que executam várias execuções de linha de comando
 * Escrever outro programa para controlar e automatizar a execução de gráficos do Dynamo e fazer coisas interessantes com os resultados desses cálculos

#### O quê
A interface de linha de comando (DynamoCLI) é um suplemento do DynamoSandbox. É um utilitário de linha de comando dos/terminal desenvolvido para fornecer a conveniência de argumentos de linha de comando para executar o Dynamo. Em sua primeira implementação, ela não é executada de forma independente, deve ser executada da pasta onde os binários do Dynamo residem, pois depende das mesmas DLLs principais do Sandbox. Ela não pode interoperar com outras compilações do Dynamo.

Há quatro formas de executar a CLI: em um prompt do Dos, de arquivos em lote do Dos e como um atalho na área de trabalho do Windows cujo caminho é modificado para incluir os indicadores de linha de comando especificados. A especificação do arquivo Dos pode ser totalmente qualificada ou relativa, e as unidades mapeadas e a sintaxe da URL também são suportadas. Ela também pode ser construída com o Mono e ser executada em Linux ou Mac no terminal.

Os pacotes Dynamo são suportados pelo utilitário. No entanto, não é possível carregar nós personalizados (dyf), apenas gráficos independentes (dyn).

Em testes preliminares, o utilitário da CLI oferece suporte a versões localizadas do Windows e é possível especificar argumentos filespec com caracteres Ascii superiores.

A CLI pode ser acessada por meio do aplicativo DynamoCLI.exe. Esse aplicativo permite que um usuário ou outro aplicativo interaja com o modelo de avaliação do Dynamo invocando o DynamoCLI.exe com uma sequência de caracteres de comandos. Isso pode ser algo assim:
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
Esse comando dirá ao Dynamo para abrir o arquivo especificado em *“C:\\someReallyCoolDynamoFile.Dyn”*, sem desenhar nenhuma IU, e executá-lo. O Dynamo será encerrado quando a execução do gráfico for concluída. 

**Novo na versão 2.1**: o aplicativo DynamoWPFCLI.exe. Esse aplicativo suporta tudo o que o aplicativo DynamoCLI.exe suporta com a adição da opção Geometria (-g). O aplicativo DynamoWPFCLI.exe está disponível somente para Windows.

#### Observações importantes

* O método preferido de interação com o DynamoCLI é por meio de uma interface de prompt do comando.
* Neste momento, será necessário executar o DynamoCLI da localização de instalação dentro da pasta [Versão do Dynamo]. A CLI precisa acessar os mesmos .dlls que o Dynamo, então ela não deve ser movida.
* Você deverá ser capaz de executar os gráficos que estão atualmente abertos no Dynamo, mas isso poderá causar efeitos colaterais indesejados.
* Todos os caminhos de arquivo são totalmente compatíveis com DOS. Portanto, os caminhos relativos e totalmente qualificados devem funcionar, mas certifique-se de colocar os caminhos entre aspas *“C:path\\to\\arquivo.dyn”* 
* O DynamoCLI é uma nova funcionalidade e atualmente está em evolução: a **CLI carrega apenas um subconjunto** de bibliotecas padrão do Dynamo no momento. Observe isso se um gráfico não for executado corretamente. Essas bibliotecas estão especificadas [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* Atualmente, nenhuma saída padrão é fornecida. Se nenhum erro for encontrado, a CLI simplesmente será fechada após a conclusão da execução.
 
#### Como

`-o` é possível abrir o Dynamo apontando para um .dyn, em um modo headless que executará o gráfico.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` pode ser usado quando o Dynamo estiver sendo executado em modo headless (quando usamos `-o` para abrir um arquivo). Esse indicador iterará todos os nós no gráfico e despejará seus valores de saída em um arquivo XML simples. Como o indicador `--ServiceMode` pode forçar o Dynamo a executar várias avaliações de gráfico, o arquivo de saída conterá valores para cada avaliação que ocorrer.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
O arquivo de saída XML teria o formato a seguir:
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` pode ser usado quando o Dynamo estiver sendo executado em modo headless (quando usamos `-o` para abrir um arquivo). Esse indicador gerará o gráfico e despejará a geometria resultante em um arquivo JSON. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
O arquivo de geometria JSON teria o formato a seguir:

 TBD – Trabalho em andamento

`-h` use essa opção para obter uma lista das possíveis opções

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

o indicador -i pode ser usado várias vezes para importar várias montagens que o gráfico que você está tentando abrir precisa para ser executado.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

o indicador -l pode ser usado para executar o Dynamo em uma configuração de localidade diferente. Mas, geralmente, a configuração de localidade não afeta os resultados do gráfico

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

o indicador --GeometryPath pode ser usado para apontar o DynamoSandbox ou a CLI para um conjunto específico de binários ASM. Use-o como

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

ou

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

o indicador -k pode ser usado para deixar o processo do Dynamo em execução até que uma extensão carregada o desligue.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

o indicador --HostName pode ser usado para identificar a variação do Dynamo associada ao hospedeiro.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

ou

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

o indicador -s pode ser usado para identificar a ID da sessão de análise do hospedeiro do Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

o indicador -p pode ser usado para identificar a ID pai da análise do hospedeiro do Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
