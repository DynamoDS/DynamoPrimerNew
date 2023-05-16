# Solicitudes de incorporación de cambios

Dynamo depende de la creatividad y el compromiso de su comunidad, y el equipo de Dynamo anima a los colaboradores a explorar posibilidades, probar ideas y solicitar la opinión de los usuarios. Aunque se fomenta la innovación, los cambios solo se incluirán si facilitan el uso de Dynamo y satisfacen las directrices definidas en este documento. No se incluirán los cambios con ventajas poco definidas.

#### Expectativas de las solicitudes de incorporación de cambios <a href="#pull-request-expectations" id="pull-request-expectations"></a>

El equipo de Dynamo espera que las solicitudes de incorporación de cambios sigan algunas directrices:

* Cumpla nuestras [normas de codificación](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) y [normas de nomenclatura de nodos](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards).
* Incluya pruebas unitarias al añadir nuevas funciones.
* Al corregir un error, añada una prueba unitaria que resalte cómo se ha interrumpido el comportamiento actual.
* No desvíe el debate del asunto que se está abordando. Cree un nuevo asunto si surge un tema nuevo o relacionado.

A continuación, se indican directrices sobre lo que no se debe hacer:

* No intente sorprendernos con grandes solicitudes de incorporación de cambios. En su lugar, presente una incidencia e inicie una conversación para que podamos ponernos de acuerdo sobre qué dirección seguir antes de que invierta una gran cantidad de tiempo.
* Confirme el código que no ha escrito. Si encuentra un código que cree que debería añadirse a Dynamo, presente una incidencia e inicie una conversación antes de continuar.
* Envíe solicitudes de incorporación de cambios que modifiquen los archivos o los encabezados relacionados con las licencias. Si cree que hay un problema con ellos, presente una incidencia; estaremos encantados de discutirlo.
* Realice adiciones a las API sin presentar una incidencia y póngase en contacto primero con nosotros.

#### Rellenar la plantilla de solicitud de incorporación de cambios<a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Al enviar una solicitud de incorporación de cambios, utilice la [plantilla por defecto](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md) para tal fin. Antes de enviar la solicitud de incorporación de cambios, asegúrese de definir claramente su finalidad y de que todo lo que se incluye en ella pueda considerarse verdadero, como se indica a continuación:

* La base de código se ha mejorado tras esta solicitud de incorporación de cambios.
* Se ha documentado según las [normas](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards).
* El nivel de pruebas de esta solicitud de incorporación de cambios es el adecuado.
* Las cadenas orientadas al usuario, si las hay, se han extraído en archivos `*.resx`.
* Se han superado todas las pruebas mediante el CI de autoservicio.
* Capture una instantánea de los cambios realizados en la interfaz de usuario, si los hay.
* Los cambios realizados en la API siguen el [versionado semántico](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) y se documentan en la sección sobre [cambios en las API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

El equipo de Dynamo asignará un revisor adecuado a su solicitud de incorporación de cambios.

#### Proceso de revisión de solicitudes de incorporación de cambios <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Una vez enviada una solicitud de incorporación de cambios, es posible que deba seguir participando durante el proceso de revisión. Tenga en cuenta los siguientes criterios de revisión:

* El equipo de Dynamo se reúne una vez al mes para revisar las solicitudes de incorporación de cambios, de las más antiguas a las más recientes.
* Si una solicitud de incorporación de cambios revisada requiere cambios por parte del propietario, este dispondrá de 30 días para responder. Si no se ha producido ninguna actividad en la solicitud de incorporación de cambios en la siguiente sesión, el equipo la cerrará, o bien otro miembro del equipo se hará cargo de ella en función de su utilidad.
* Las solicitudes de incorporación de cambios deben utilizar la plantilla por defecto de Dynamo para tal fin.
* Las solicitudes de incorporación de cambios en las que no se hayan rellenado por completo las plantillas de Dynamo con toda la información indicada no se revisarán.

#### Selección de confirmaciones de Dynamo Revit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Dado que hay varias versiones de Revit disponibles en el mercado, es posible que tenga que seleccionar los cambios en ramificaciones específicas de la versión de DynamoRevit para que las diferentes versiones de Revit puedan recopilar la nueva funcionalidad. Durante el proceso de revisión, los colaboradores serán responsables de seleccionar las confirmaciones revisadas para las otras ramificaciones de DynamoRevit especificadas por el equipo de Dynamo.
