# Configurar Dynamo Player en Forma


Para utilizar Dynamo con Forma, tiene estas dos opciones: puede usar Dynamo como servicio (DaaS) basado en la nube o la versión de escritorio de Dynamo. Cada una tiene sus ventajas en función de lo que desee hacer, así que antes de empezar, considere la opción que mejor se adapte a sus necesidades. Sin embargo, tenga en cuenta que puede alternar entre estas opciones en cualquier momento.

**Comparación entre Dynamo como servicio y la versión de escritorio de Dynamo**

<table><thead><tr><th>Dynamo como servicio</th><th>Versión de escritorio</th><th data-hidden></th></tr></thead><tbody><tr><td>Fácil configuración.</td><td>Proceso de instalación en varios pasos.</td><td></td></tr><tr><td>No es necesario instalar Dynamo ni tenerlo abierto.</td><td>Es necesario tener abierto Dynamo.</td><td></td></tr><tr><td>No reconoce un gráfico que ya se haya abierto en Dynamo</td><td>Abra un gráfico en el Reproductor que ya se haya abierto en Dynamo con solo hacer clic en un botón.</td><td></td></tr><tr><td>No se puede utilizar Python.</td><td>Se puede utilizar Python.</td><td></td></tr><tr><td>Solo se pueden utilizar paquetes autorizados.</td><td>Se puede utilizar cualquier paquete.</td><td></td></tr></tbody></table>

En esta página, explicaremos cómo configurar ambas opciones.

### Configuración de Dynamo como servicio

Dynamo en Forma (beta) está disponible actualmente como versión beta abierta de acceso anticipado, lo que significa que las funciones y la interfaz de usuario pueden cambiar con frecuencia.

En primer lugar, instalaremos Dynamo Player en Forma.

1. En el sitio de Forma, vaya a **Extensions** en el barra lateral izquierda y haga clic en **Add extension**. Se abre el sitio web de Autodesk App Store.
2. Busque Dynamo y añada la versión beta de Dynamo Player. Lea la limitación de responsabilidades y haga clic en **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Dynamo Player está ahora disponible en sus extensiones. Haga clic en él para abrirlo.
4. Ya puede utilizar Dynamo Player.

### Configuración de la versión de escritorio de Dynamo

Para utilizar la versión de escritorio de Dynamo, necesitará Dynamo, ya sea como Sandbox independiente o conectado a Revit o Civil 3D. También necesitará el paquete DynamoFormaBeta.

#### Revit

Siga estas instrucciones para configurar Dynamo en Revit y el paquete DynamoFormaBeta.

1. Asegúrese de tener instalado Revit 2024.1 o una versión posterior.
2. Abra Dynamo en Revit mediante Gestionar > Dynamo.
3. En Dynamo, instale el paquete DynamoFormaBeta. Vaya a Paquetes > Package Manager y, a continuación, busque DynamoFormaBeta.
   1. Si dispone de Revit 2024, instale el paquete DynamoFormaBeta for 2.x.
   2. Si dispone de Revit 2025, instale el paquete DynamoFormaBeta.

#### Civil 3D

Siga estas instrucciones para configurar Dynamo en Civil 3D y el paquete DynamoFormaBeta.

1. Asegúrese de tener instalado Civil 3D 2024.1 o una versión posterior.
2. Abra Dynamo en Civil 3D mediante Gestionar > Dynamo.
3. En Dynamo, instale el paquete DynamoFormaBeta. Vaya a Paquetes > Package Manager y, a continuación, busque DynamoFormaBeta.
   1. Si tiene Civil 3D 2024, instale el paquete DynamoFormaBeta for 2.x.
   2. Si tiene Civil 3D 2025, instale el paquete DynamoFormaBeta.

#### Dynamo Sandbox

Siga estas instrucciones para instalar Dynamo Sandbox y el paquete DynamoFormaBeta.

1. Descargue Dynamo 2.18.0 o superior desde las [compilaciones de Dynamo](https://dynamobuilds.com/). Para obtener la mejor experiencia, elija la última de las versiones más estables, que aparece en la parte superior.
   1. Las versiones diarias son versiones de desarrollo y pueden incluir funciones incompletas o en curso.
2. Extraiga Dynamo mediante [7zip](https://7zip-es.updatestar.com/) en la carpeta que elija.
3. Ejecute DynamoSandbox.exe desde la carpeta de instalación de Dynamo.
4. En Dynamo, instale el paquete DynamoFormaBeta. Vaya a Paquetes > Package Manager y, a continuación, busque DynamoFormaBeta.
   1. Si dispone de Dynamo 2.x, instale el paquete DynamoFormaBeta for 2.x.
   2. Si dispone de Dynamo 3.x, instale el paquete DynamoFormaBeta.

Una vez que Dynamo esté instalado, podrá utilizarlo con Forma. Al ejecutar la opción de escritorio de Dynamo en Forma, deberá tener Dynamo abierto para poder utilizar la extensión de Dynamo Player.

#### Acceso a la versión de escritorio en Forma

En primer lugar, instalaremos Dynamo Player en Forma.

1. En el sitio de Forma, vaya a **Extensions** en el barra lateral izquierda y haga clic en **Add extension**. Se abre el sitio web de Autodesk App Store.
2. Busque Dynamo y añada la versión beta de Dynamo Player. Lea la limitación de responsabilidades y haga clic en **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Dynamo Player está ahora disponible en sus extensiones. Haga clic en él para abrirlo.
4. En la parte superior, haga clic en Desktop para acceder a la versión de escritorio de Dynamo.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. Ya puede utilizar Dynamo Player. Si ya tiene un gráfico abierto en Dynamo, haga clic en "Open" debajo de **Connected graph** para verlo en el Reproductor.
