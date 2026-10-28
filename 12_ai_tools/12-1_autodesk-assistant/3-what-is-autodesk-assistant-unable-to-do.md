# O que o Autodesk Assistant ainda não consegue fazer

### Atualizar pacotes

O Autodesk Assistant ainda não consegue baixar nem instalar pacotes no Dynamo

![](../../.gitbook/assets/Aa_Update.jpg)

Como mostrado na imagem acima, o Autodesk Assistant não consegue modificar, instalar ou desinstalar pacotes externos associados ao Dynamo. Ele pode fornecer instruções para que você os instale.


### Ler e gravar arquivos diretamente

O Autodesk Assistant funciona com arquivos de gráficos do Dynamo: ele pode abrir um gráfico, inseri-lo na tela, executá-lo e salvar o trabalho. Ele não consegue ler nem gravar outros arquivos no disco rígido por conta própria.

O exemplo abaixo mostra geometria criada na tela. Solicita-se ao Autodesk Assistant que extraia e exporte como JSON os pontos que compõem a geometria. Embora o Autodesk Assistant não consiga gerar esse JSON diretamente, ele sugere a criação de um script do Dynamo para isso.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Esse é o padrão que deve ser lembrado. Tudo o que está fora do gráfico é acessado por meio de nós, não pelo próprio Assistant.

### Alterar o modelo do Revit sem um gráfico

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

O Autodesk Assistant para Dynamo não tem conexão direta com o modelo do Revit. Se você solicitar que ele edite o modelo, como faria com o Assistant no Revit, ele informará que não é capaz de realizar essa tarefa.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Ele ainda pode alterar o modelo do Revit. Ele faz isso como você faria: insere nós do Revit, conecta-os entre si e, quando o gráfico é executado, o modelo é modificado.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Essa forma de trabalhar implica duas coisas:

- **Nada muda até que o gráfico seja executado.** É possível ler o gráfico e ajustar os valores primeiro. O modelo só é alterado quando o gráfico é executado, seja por você, seja pelo Assistant como parte da solicitação.
- **A alteração é reproduzível.** O gráfico permanece na tela. Salve-o para executar a mesma tarefa novamente em outro modelo ou compartilhá-lo com sua equipe.

Se você quiser fazer uma edição pontual e direta, sem envolver um gráfico, use o Autodesk Assistant no Revit.
