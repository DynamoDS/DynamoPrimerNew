# Executar scripts Python em nós sem toque (C#) 

### Executar scripts Python em nós sem toque (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Se você estiver familiarizado com a escrita de scripts no Python e desejar mais funcionalidade dos nós padrão do Dynamo Python, poderemos usar o recurso Sem toque para criar nossos próprios. Vamos começar com um exemplo simples que nos permite passar um script Python como uma sequência de caracteres para um nó Sem toque, onde o script é executado e um resultado é retornado. Este estudo de caso será baseado nos tutoriais virtuais e nos exemplos na seção Introdução. Consulte os exemplos se você for completamente novo na criação de nós Sem toque.

![Um nó Sem toque que executará uma sequência de caracteres de script Python](images/python-case-study.png)

> Um nó Sem toque que executará uma sequência de caracteres de script Python

#### Mecanismo Python <a href="#python-engine" id="python-engine"></a>

Esse nó depende de uma instância do mecanismo de scripts IronPython. Para fazer isso, precisamos fazer referência a algumas montagens adicionais. Siga as etapas abaixo para configurar um modelo básico no Visual Studio:

* Criar um novo projeto de classe do Visual Studio
* Adicionar uma referência ao `IronPython.dll` localizado em `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`
* Adicionar uma referência ao `Microsoft.Scripting.dll` localizado em `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`
* Incluir as declarações `IronPython.Hosting` e `Microsoft.Scripting.Hosting` `using` na classe
* Adicionar um construtor vazio privado para evitar que um nó adicional seja adicionado à biblioteca do Dynamo com nosso pacote
* Criar um novo método que toma uma única sequência de caracteres como um parâmetro de entrada
* Dentro desse método, instanciaremos um novo mecanismo Python e criaremos um escopo de script vazio. Você pode imaginar esse escopo como as variáveis globais dentro de uma instância do interpretador Python
* Em seguida, chamar `Execute` no mecanismo que está passando a sequência de entrada e o escopo como parâmetros
* Por fim, recuperar e retornar os resultados do script chamando `GetVariable` no escopo e passando o nome da variável do script Python que contém o valor que você está tentando retornar. (Consulte o exemplo abaixo para obter detalhes adicionais)

O código a seguir fornece um exemplo para a etapa mencionada acima. A compilação da solução criará um novo `.dll` localizado na pasta bin de nosso projeto. Agora, é possível importar este `.dll` para o Dynamo como parte de um pacote ou navegar para `File < Import Library...`

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

O script Python está retornando a variável `output`, o que significa que precisaremos de uma variável `output` no script Python. Use esse script de amostra para testar o nó no Dynamo. Se você já tiver usado o nó Python no Dynamo, o seguinte deverá ser familiar. Para obter mais informações, consulte a [seção Python do manual](http://dynamoprimer.com/en/09\_Custom-Nodes/9-4\_Python.html).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Várias saídas <a href="#multiple-outputs" id="multiple-outputs"></a>

Uma limitação dos nós Python padrão é que eles têm somente uma única porta de saída, portanto, se desejarmos retornar vários objetos, deveremos criar uma lista e recuperar cada objeto. Se modificarmos o exemplo acima para retornar um dicionário, poderemos adicionar quantas portas de saída desejarmos. Consulte a seção Retornar vários valores em “Aprofundar o conhecimento sobre o nó Sem toque” para obter informações mais específicas sobre os dicionários.

![Esse nó está nos permitindo retornar o volume do cuboide e seu centroide.](images/python-multi-case-study.png)

> Esse nó está nos permitindo retornar o volume do cuboide e seu centroide.

Vamos modificar o exemplo anterior com estas etapas:

* Adicionar uma referência ao `DynamoServices.dll` do gerenciador de pacotes do NuGet
* Além das montagens anteriores, incluir `System.Collections.Generic` e `Autodesk.DesignScript.Runtime`
* Modificar o tipo de retorno em nosso método para retornar um dicionário que conterá nossas saídas
* Cada saída deve ser individualmente recuperada do escopo (considere a possibilidade de configurar um loop simples para conjuntos maiores de saídas)

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

Também adicionamos uma variável de saída (`output2`) ao script python de amostra. Lembre-se de que essas variáveis podem usar qualquer convenção de nomenclatura Python legal, a saída foi usada estritamente para fins de clareza neste exemplo.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```
