# thinkingBig
A project structure to manage complex environments on Android

¿Cómo organizar un proyecto que permita crear diferentes apks con un negocio común aunque altamente variable?

Ejemplos:

 * Tu empresa provee un conjunto de servicios (mensajería, noticias, formularios...) que adapta a las necesidades
   de sus clientes. Estos clientes pueden ser muy diferentes, una clínica dental, un ayuntamiento, una empresa de
   estintores..., y pueden estar orientadas a un público también diferente, por ejemplo, a los usuarios de la clínica, 
   a los habitantes de una ciudad o a los trabajadores de una empresa.

 * Trabajas para un grupo energético al que pertenecen un conjunto de empresas del mismo sector. Aunque pertenecen
   al mismo grupo, estas empresas tienen entidad de marca, y sensu stricto compiten unas con otras en el mercado.

 * Trabajas para una entidad bancaria en proceso de expansión. Cada mercado nuevo al que el banco se expande 
   se materializa en una nueva aplicación adaptada a ese mercado.

Como punto de partida asumiremos que cada uno de estos escenarios se puede desarrollar en un contexto altamente 
volatil/variable. Todos tienen una lógica de negocio de común, no obstante, esa lógica puede variar desde un 
primer momento o cambiar con el tiempo. Esos cambios pueden afectar a la "vista", al "dominio" y a los "datos".

Una de las soluciones más habituales a la hora de enfrentarse a estos escenarios es usar las buildVariants que 
ofrece el plugin de Gradle de Android (AGP). Desde esta perspectiva el proyecto tendría un único punto de entrada
(módulo application) y n variantes de compilación. Cada variante de compilación (buildVariant) estaría formada por
la únión de un buildType y un flavor. Cada flavor representaría uno de los segmentos en los que tu 
empresa ha decidido dividir su negocio (los clientes en el caso de la empresa que provee servicios de mensajería...,
cada una de las marcas del grupo energético o cada uno de los mercados en los que el banco tiene presencia). 
El código común estaría en el sourceSet de "main" y el código específico de cada "segmento" en el flavor correspondiente.

En este proyecto vamos a abordar una solución alternativa a las buildVariants basada en la composición y en la
hipermodularización de las capas en las que dividamos nuestro negocio. En lugar de crear un único módulo application
con n flavors, crearemos un módulo application por segmento, y usaremos la hipermodularización (microservicios) para 
separar el código común del específico del segmento.

Asumimos una serie de premisas que queremos contrastar en relación con la hipermodularización:

Cuanto mayor sea la modularización del proyecto:
 * menor será la responsabilidad de cada módulo
 * menor será la duplicación de código
 * mayor será la reutilización del código
 * mayor será la escalabilidad del proyecto