# Automatización del proceso de análisis de Barkibu

## Contexto del problema:

Buscamos integrar automatización en el flujo de trabajo de la aplicación móvil de Barkibu, para poder reducir la carga del equipo de especialistas humanos. Para dar contexto a los puntos donde se podría implementar dicha automatización, hay que recordar que el ritmo de trabajo usual de la aplicación es el siguiente:

-   Los clientes que ya tienen contratada una póliza de seguros suben sus documentos relacionados con la visita al veterinario a la aplicación. Entre dichos documentos se pueden encontrar la factura del veterinario, el informe del veterinario para comprobar qué partes de la visita que puedan estar cubiertas por la póliza, y un historial de patologías del animal.

-   El equipo de especialistas de Barkibu analizan los documentos para comprobar qué partes de la visita están cubiertas por la póliza, si la patología puede ser afectada por condiciones previas, y a cuánto ascendería la cantidad a reembolsar al cliente.

## Puntos donde se podría implementar la automatización del proceso

Analizando el problema, creo que hay 3 puntos principales donde podría implementarse la automatización:

1. Automatizar el proceso entero en la aplicación, permitiendo que el programa analice los documentos, haga el proceso que haría el equipo de especialistas, y se lo ofrezca al usuario.
2. Automatizar la lectura y análisis de los documentos, permitiendo que el programa analice los datos, los resuma de forma sencilla y más legible para el equipo de especialistas, y que ellos tomen las decisiones a nivel de cuánto dinero se podría reembolsar al cliente.
3. Que el programa haga un análisis completo de los datos y haga un estimado de todos los factores, pero en lugar de presentárselos al cliente se lo presente al equipo de especialistas para que comprueben primero la veracidad y la calidad del análisis, antes de presentárselo al cliente como un análisis aceptado por el equipo de especialistas.

## Recomendación de qué ruta seguir:

En mi opinión, creo que la opción que debemos implementar es la tercera. Empezaré explicando por qué creo que las otras dos opciones no solucionarían nuestro problema de base, que es reducir la carga de trabajo del equipo de especialistas.

Empezando por la primera opción, si bien es cierto que podríamos ahorrar mucho tiempo de respuesta al usuario, y podríamos reducir la carga de trabajo en un principio al hacer la IA el análisis, es muy probable que un porcentaje muy relevante de la base de usuarios no aceptase el análisis proporcionado por la aplicación, y quisiese reclamar un análisis realizado por el equipo de analistas humanos, por miedo a un error por parte del análisis de la IA. Esto no conseguiría mejorar la carga de trabajo del equipo humano, y probablemente tendríamos que empezar desde cero.

La segunda opción podría ser una buena solución debido a que podría simplificar parte del trabajo del equipo al cortar la mitad del proceso que tendrían que hacer ellos, siendo esta la parte de analizar los documentos. Si esta parte es la que más tiempo consume al equipo podría ser una elección completamente satisfactoria para nuestro fin, pero creo que podríamos ser más efectivos con la tercera opción.

La tercera opción, preparar un analista de datos que pueda hacer un análisis preliminar de los documentos y hacer un estimado de los datos, que luego podamos presentar al equipo de especialistas para que le den el visto bueno, creo que es la elección adecuada para nuestro proceso.

## Explicación en detalle de la solución sugerida:

Creo que la elección de automatizar el proceso inicial, pero hacer que el resultado presentado al cliente sea verificado por el proceso, es la solución que nos da el mayor número de ventajas.

Primero de todo, en el aspecto de disminuir la carga de trabajo del equipo de especialistas, esta opción nos permite reducir la cantidad de tiempo que deben pasar con cada caso individual, al solamente tener que revisar el análisis de la aplicación. Por supuesto la última palabra sobre cada caso la tendría el propio equipo, pudiendo ellos revisar todos los datos desde cero y hacer el análisis como se está haciendo actualmente para casos puntuales en los que encuentren errores en el análisis, o con circunstancias especiales, pero una vez el analista esté entrenado con los datos relevantes para poder hacer análisis de forma rápida y con un grado aceptable de calidad el equipo pasaría muchísimo menos tiempo en cada caso individual.

Continuando con la posibilidad de escalar el sistema, al poder entrenar nosotros el sistema con los documentos que nosotros le alimentemos, y podamos educarlo con parámetros importantes para nosotros (por ejemplo, los datos de cada póliza de seguros para saber qué enfermedades se encuentran cubiertas) esto nos permite poder ir integrando nuevos parámetros relevantes con facilidad al programa.

Otro punto a tener en cuenta es la interacción que tenga el usuario con el nuevo sistema. Si bien quizás no permita hacer un análisis casi instantáneo de cara al usuario como nos permitiría la primera opción (porque podríamos integrarlo de forma que le dé al usuario su análisis de forma casi instantánea al no depender de que le den el visto bueno), ciertamente haría que el tiempo de respuesta entre la solicitud del usuario y la respuesta sea mucho menor, al saltarse el paso que lleva más tiempo, que es el análisis por parte del equipo de todos los datos. Además, los usuarios son mucho más propensos a confiar en el análisis si este ha sido aprobado por un especialista humano antes de que se les presente, en lugar de un análisis realizado sin supervisión por parte de una IA, lo que además mejoraría la experiencia del usuario y su propensión a utilizar nuestro sistema.

## Contras de la solución escogida:

Si bien esta opción tiene varios puntos positivos, tiene unos elementos en contra que hay que tener en cuenta:

### Carga de tiempo de entrenar el modelo de forma acorde a nuestras necesidades:

Para poder hacer análisis relevantes con los datos proporcionados, debemos seguir un proceso de educación del modelo del analista con nuestra documentación, detalles relevantes de nuestro contexto (las pólizas disponibles para nuestros clientes con sus limitaciones, por ejemplo) y además pasar un tiempo curando sus datos para poder educarlo y que su análisis sea de ayuda al equipo. Esta es la mayor contra de esta opción, al necesitar una cantidad de tiempo relevante en la que el sistema no estaría ayudando de forma activa mientras está siendo implementado y alimentado.

### Posibles imprecisiones debido a mala calidad de los documentos proporcionados:

Es posible que si el cliente proporciona documentos de mala calidad (fotografías sacadas con el móvil, por ejemplo) el programa no analice de forma correcta los documentos. Esto es prevenible pidiéndole al cliente que envíe los documentos de nuevo con mayor calidad, pudiendo incluso añadir un paso de "curado" de los documentos, donde se analiza su calidad primero antes de proceder con la solicitud.

## Ejecución de la implementación

La estructura necesaria consiste de lo siguiente:

-   La aplicación móvil de Barkibu, modificada
-   Un servidor que recoja la petición del cliente, la categorice y la procese para hacer un análisis inicial con un modelo de lenguaje
-   Un portal donde nuestro equipo de especialistas pueda ver la petición, el análisis inicial del modelo de lenguaje, y en caso necesario pueda hacer todo el análisis con los documentos proporcionados por el cliente.

Partiendo de la aplicación de Barkibu, solamente tendríamos que modificarla para enviar los datos a nuestro servidor de análisis en vez de al servidor donde se estén enviando para su revisión por el equipo de analistas. No estoy seguro de si en la actualidad la respuesta de la solicitud le es enviada al cliente por correo electrónico o por la aplicación, pero para este ejemplo asumo que vamos a enviárselo a la aplicación, así que también deberíamos añadirle la funcionalidad para poder recibir la solicitud si es el caso.

En el servidor de análisis, propongo primero procesar los documentos para tener sus datos en texto en lugar de en formato pdf, y luego implementar un modelo de lenguaje que eduquemos para utilizar tanto nuestros datos de contexto (las pólizas disponibles con sus datos) como para ir añadiendo documentación relevante para el proceso (diferentes modelos de facturas, análisis e historiales que puedan proporcionarnos los clientes). El modelo específico que utilicemos es dependiente de muchos factores, pero QWEN en su último modelo es un buen punto de partida.

El portal al que tendría acceso nuestro equipo podría ser un portal web con una funcionalidad similar a Jira, donde pudiesen ver las solicitudes en tiempo real, actualizarlas, ver su estado y modificarlas según viesen conveniente. Se podría construir fácilmente conectando el output del servidor de análisis a la base de datos de las solicitudes, y de forma que parsee el contenido directamente al formato que utilicemos en el portal. Además, esto sirve para poder continuar teniendo categorizadas las solicitudes para labores de mantenimiento y de archivación.

## Usos adicionales para la estructura de datos:

Uno de los beneficios de entrenar un modelo de lenguaje para analizar los documentos que nos envíen los clientes es que se podría implementar para funcionalidades adicionales. Entre estas funcionalidades se encuentran, por ejemplo:

-   Crear un recomendador de pólizas basado en el perfil de la mascota que nos pueda proporcionar el cliente, de forma que pueda recomendar una u otra según los datos que nos dé (su historial, análisis de su veterinario, etc)
-   Implementar un perfil de la mascota dentro de la aplicación, y que con los datos proporcionados la aplicación pueda dar avisos al cliente, como recordatorios personalizados.
-   Siguiendo con la idea de crear un perfil de la propia mascota, podríamos hacer que la aplicación le recomiende contenido relevante al cliente que coincida con lo que pueda necesitar su mascota (artículos, videos, etc).

Todos estos casos de uso adicionales se verían beneficiados por el trabajo previo de crear y educar el modelo, y si continuásemos educándolo con datos nuevos eso continuaría mejorando la precisión y fiabilidad del modelo para los datos.
