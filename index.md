Este artículo es una traducción del siguiente artículo publicado originalmente en inglés: [Reverse Engineering the source code of the BioNTech/Pfizer SARS-CoV-2 Vaccine](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). La traducción está hecha con ayuda de [Google Translate](https://translate.google.es/). Si ves una errata, eres bienvenid@ a hacer una PR. 
 
El artículo aquí descrito proporciona descripción de la vacuna de BioNTech/Pfizer utilizando una analogía con las computadoras. Creo que es una perspectiva muy interesante que ayuda a comprender la complejidad y la potencia de este tipo de tecnología. 


# Ingeniería inversa del código fuente de la vacuna BioNTech / Pfizer SARS-CoV-2

¡Bienvenidos! En esta publicación, analizaremos, carácter por carácter, el código fuente<sup>[1](#myfootnote1)</sup> de la vacuna de ARNm del SARS-CoV-2 de BioNTech / Pfizer.

>Quiero agradecer al gran número de personas que dedicaron tiempo a la lectura de este artículo para mejorar su legibilidad y corregir errores. Sin embargo, todos los errores siguen siendo míos, pero me encantaría escucharlos rápidamente en bert@hubertnet.nl o @PowerDNS_Bert

Esto que digo puede resultar algo discordante: la vacuna es un líquido que se inyecta en el brazo. ¿Cómo podemos hablar entonces de código fuente?


Esta es una buena pregunta, así que comencemos con una pequeña parte del mismo código fuente de la vacuna BioNTech / Pfizer, también conocida como [BNT162b2](https://en.wikipedia.org/wiki/Tozinameran) , también conocida como Tozinameran también conocida como [Comirnaty](https://twitter.com/PowerDNS_Bert/status/1342109138965422083).

![](https://berthub.eu/articles/bnt162b2.png)

*Primeros 500 caracteres del ARNm de BNT162b2. Fuente: [Organización Mundial de la Salud](https://mednet-communities.net/inn/db/media/docs/11889.doc).*

El "corazón" de la vacuna de ARNm BNT162b tiene el código digital que aparece en esta imagen de arriba. En total, tiene 4284 caracteres, por lo que cabría en unos cuantos tweets. Al comienzo del proceso de producción de la vacuna, alguien cargó este código en una impresora de ADN (sí), que luego convirtió estos bytes almacenenados en su disco duro en moléculas de ADN reales.

![](https://berthub.eu/articles/bioxp-3200.jpg)

*Una impresora de ADN [Codex DNA](https://codexdna.com/products/bioxp-system/) BioXp 3200*

De una máquina así salen pequeñas cantidades de ADN, que después de mucho procesamiento biológico y químico terminan como ARN (hablaré sobre esto de nuevo más adelante) en el vial de la vacuna. Una dosis de 30 microgramos en realidad contiene 30 microgramos de ARN. Además, tiene un sistema inteligente de empaquetado de lípidos (grasos) que lleva el ARNm a nuestras células.

El ARN es la versión volátil de "memoria de trabajo" del ADN. El ADN es como el disco duro de la biología. El ADN es muy duradero, internamente redundante y muy seguro. Pero al igual que las computadoras no ejecutan código directamente desde una unidad de disco duro, antes de hacer algo, el código se copia en un sistema más rápido, más versátil pero mucho más frágil.

Para las computadoras, esto es la memoria RAM, para biología es el ARN. El parecido es sorprendente. A diferencia del dico duro, la memoria RAM se degrada muy rápidamente a menos que se le trate con cariño. La razón por la que la vacuna de ARNm de Pfizer/BioNTech debe almacenarse en el congelador ultra frío es la misma: el ARN es una flor muy frágil.

Cada carácter de ARN pesa del orden de 0,53·10⁻²¹ gramos, lo que significa que hay 6·10¹⁶ caracteres en una sola dosis de vacuna de 30 microgramos. Expresado en bytes, esto es alrededor de 25 petabytes, aunque hay que decir que se trata de alrededor de 2000 mil millones de repeticiones de los mismos 4284 caracteres. El contenido informativo real de la vacuna es de poco más de un kilobyte. El [propio SARS-CoV-2](https://www.ncbi.nlm.nih.gov/projects/sviewer/?id%3DNC_045512%26tracks%3D%5Bkey:sequence_track,name:Sequence,display_name:Sequence,id:STD649220238,annots:Sequence,ShowLabel:false,ColorGaps:false,shown:true,order:1%5D%5Bkey:gene_model_track,name:Genes,display_name:Genes,id:STD3194982005,annots:Unnamed,Options:ShowAllButGenes,CDSProductFeats:true,NtRuler:true,AaRuler:true,HighlightMode:2,ShowLabel:true,shown:true,order:9%5D%26v%3D1:29903%26c%3Dnull%26select%3Dnull%26slim%3D0) "pesa" alrededor de 7,5 kilobytes.

## Algo de transfondo

El ADN es un código digital. A diferencia de las computadoras, que usan 0 y 1, la vida usa A, C, G y U/T (los 'nucleótidos', 'nucleósidos' o 'bases').

En las computadoras almacenamos el 0 y el 1 como la presencia o ausencia de una carga, o como una corriente, como una transición magnética, o como un voltaje, o como una modulación de una señal, o como un cambio en la reflectividad. O, en resumen, el 0 y el 1 no son una especie de concepto abstracto: viven como electrones y en muchas otras encarnaciones físicas.

En la naturaleza, A, C, G y U/T son moléculas, almacenadas como cadenas en el ADN (o ARN).

En las computadoras, agrupamos 8 bits en un byte, y el byte es la unidad típica de datos que se procesan.

La naturaleza agrupa 3 nucleótidos en un codón, y este codón es la unidad típica de procesamiento. Un codón contiene 6 bits de información (2 bits por carácter de ADN, 3 caracteres = 6 bits. Esto significa 2⁶ = 64 valores de codón diferentes).

Bastante digital hasta ahora. En caso de duda, consulte el documento de la OMS con el código digital para comprobarlo usted mismo.

>Algunas lecturas adicionales están disponibles [aquí](https://berthub.eu/articles/posts/what-is-life/): este enlace ('¿Qué es la vida?') puede ayudar a entender el resto de esta página. O, si te gusta el video, tengo [dos horas para ti](https://berthub.eu/dna).

## Entonces, ¿qué hace ese código?


La idea de una vacuna es enseñarle a nuestro sistema inmunológico cómo combatir un patógeno, sin que enfermemos nosotros mismos. Históricamente, esto se ha hecho inyectando un virus debilitado o incapacitado (atenuado), más un 'adyuvante' para asustar a nuestro sistema inmunológico y ponerlo en acción. Esta era una técnica decididamente análoga que involucraba miles de millones de huevos (o insectos). También requirió mucha suerte y mucho tiempo. A veces, también se utilizó un virus diferente (no relacionado).

Una vacuna de ARNm logra lo mismo ('educar a nuestro sistema inmunológico') pero de una manera similar al láser. Y lo digo en ambos sentidos: muy limitado pero también muy poderoso.

Pues así es como funciona. La inyección contiene material genético volátil que describe la famosa proteína 'Spike' o de pico del SARS-CoV-2. Mediante medios químicos muy ingeniosos, la vacuna logra introducir este material genético en algunas de nuestras células.

Luego, estos comienzan a producir proteínas de pico de SARS-CoV-2 en cantidades lo suficientemente grandes como para que nuestro sistema inmunológico entre en acción. Enfrentado con las proteínas Spike y (lo que es más importante) los signos reveladores de que las células han sido "atacadas", nuestro sistema inmunológico desarrolla una respuesta poderosa contra múltiples aspectos de la proteína Spike **y** su proceso de producción.

Y esto es lo que lleva a la vacuna a tener una eficacia del 95%.

## El código fuente!

[Empecemos por el principio, un muy buen punto de partida](https://youtu.be/jp0opnxQ4rY?t%3D8). El documento de la OMS tiene esta imagen que es muy útil:

![](https://berthub.eu/articles/vaccine-toc.png)

Esta es una especie de tabla de contenido. Comenzaremos con la 'gorra', en realidad representada como un pequeño sombrero.

Al igual que no se puede introducir instruccciones directamente en el código de una computadora y ejecutarlo, el sistema operativo biológico requiere encabezados, y también enlazadores y cosas como convenciones a la hora de hacer llamadas.

El código de la vacuna comienza con los siguientes dos nucleótidos:

```
GA
```

Esto se puede comparar direcatemente con los archivos ejecutables de MS-DOS y Windows que comienzan con las letras `MZ` , o con los scripts de UNIX que comienzan con `#!`. Tanto en la vida como en los sistemas operativos, estos dos caracteres no describen ningun proceso biológico o instrucción. Pero tienen que estar ahí porque de lo contrario no pasaría nada.

La 'gorra' del ARNm tiene varias funciones. Por un lado, indica que el código (o conjunto de caracteres) que vienen después provienen del núcleo de la célula. En nuestro caso, por supuesto que no, nuestro código proviene de una vacuna. Pero no hay decirle eso a la célula! La "gorra" hace que nuestro código parezca legítimo, lo que lo protege de ser destruido.

Los dos nucleótidos iniciales `GA` también son ligeramente diferentes en términos químicos del resto del ARN. En este sentido, los dos nucleótidos `GA` contienen algun tipo de señalización *fuera de banda*.


## La "región sin traducir 5' "

Algo de jerga aquí. Las moléculas de ARN solo se pueden leer en una dirección. De manera algo confusa, la parte donde comienza la lectura de una molécula de ARN se le llama 5' o *cinco prima*. Y la parte donde se termina la lectura de una molécula de ARN se le llama 3' o *tres prima*.

La vida consiste en proteínas (o cosas hechas por proteínas). Y estas proteínas se describen en ARN. Cuando el ARN se convierte en proteínas, esto se llama **traducción**.

Aquí tenemos la región 5' *no traducida* ("5' UTR"), por lo que estos bits/caracteres no aparecen dentro de la proteína:

```
GAAΨAAACΨAGΨAΨΨCΨΨCΨGGΨCCCCACAGACΨCAGAGAGAACCCGCCACC
```

Aquí encontramos nuestra primera sorpresa. Los caracteres normales del ARN son A, C, G y U. U también se conoce como 'T' en el ADN. Pero aquí encontramos un Ψ, ¿qué está pasando?

Este es uno de los aspectos excepcionalmente más inteligentes de la vacuna. Nuestro cuerpo ejecuta un potente sistema antivirus ("el original"). Por esta razón, las células son muy poco entusiastas con el ARN extraño y se esfuerzan mucho por destruirlo antes de que haga algo.

Esto es un problema para nuestra vacuna: debe escabullirse de nuestro sistema inmunológico. Durante muchos años de experimentación, se descubrió que si la U en el ARN se reemplaza por una molécula ligeramente modificada, nuestro sistema inmunológico pierde el interés en destuirlo. Es así, de verdad!

Entonces, en la vacuna BioNTech/Pfizer, cada U ha sido reemplazada por *1-metil-3'-pseudouridililo*, denotado por Ψ. Lo realmente inteligente es que, aunque este reemplazo Ψ aplaca (calma) nuestro sistema inmunológico, es aceptado como una U normal por partes relevantes de la célula!

En seguridad informática también conocemos este truco: a veces es posible transmitir una versión ligeramente dañada de un mensaje que confunde los firewalls y las soluciones de seguridad, pero que aún es aceptado por los servidores finales, que luego pueden ser pirateados.


Ahora estamos cosechando los beneficios de la investigación científica fundamental realizada en el pasado. Los [descubridores](https://twitter.com/PennMedicine/status/1341766354232365059) de esta técnica tuvieron que luchar para conseguir financiar [su trabajo](https://www.statnews.com/2020/11/10/the-story-of-mrna-how-a-once-dismissed-idea-became-a-leading-technology-in-the-covid-vaccine-race/) y ser aceptado. Todos deberíamos estar muy agradecidos y estoy seguro de que [los premios Nobel llegarán a su debido tiempo](https://twitter.com/PowerDNS_Bert/status/1329861047168225281).

>Mucha gente se ha preguntado si los virus también podrían utilizar la técnica Ψ para vencer a nuestro sistema inmunológico. En resumen, esto es extremadamente improbable. La vida simplemente no tiene la maquinaria para construir nucleótidos de *1-metil-3'-pseudouridililo*. Los virus dependen de la maquinaria de la vida para reproducirse, y esta *infraestructura* simplemente no existe. Las vacunas de ARNm se degradan rápidamente en el cuerpo humano y no hay posibilidad de que el ARN modificado en Ψ se replique con el Ψ todavía allí. [“No, de verdad, las vacunas de ARNm no afectarán su ADN”](https://www.deplatformdisease.com/blog/no-really-mrna-vaccines-are-not-going-to-affect-your-dna) también es una buena lectura.

Ok, volvamos a la zona *no traducida* 5'. ¿Qué hacen estos 51 caracteres? Como todo en la naturaleza, casi nada tiene una función clara.

Cuando nuestras células necesitan traducir ARN en proteínas, esto se hace utilizando una máquina llamada *ribosoma*. El ribosoma es como una impresora 3D de proteínas. Ingiere una cadena de ARN y, en base a eso, emite una cadena de aminoácidos, que luego se pliegan en una proteína.

![](https://berthub.eu/articles/protein-short.mp4)

Esto es lo que vemos que sucede en el video de aquí arriba. La cinta negra en la parte inferior es ARN. La cinta que aparece en la parte verde es la proteína que se está formando. Las cosas que entran y salen son aminoácidos junto con adaptadores para que encajen en el ARN.

Este ribosoma necesita situarse físicamente en la hebra de ARN para que funcione. Una vez situado, puede comenzar a formar proteínas basadas en el nuevo ARN que va ingiriendo. A partir de esto, uno puede imaginar como el ribosoma no puede leer la parte de la hebra del ARN donde *aterriza* al principio. Esta es solo una de las funciones de estas zonas *no traducidas*: ser la pista de aterrizaje del ribosoma. Las zonas *no traducidas* son como una "introducción".

Además de esto, las zonas *no traducidas* también contienen metadatos<sup>[2](#myfootnote2)</sup>: ¿cuándo debe realizarse la traducción? ¿Y cuánto? Para la vacuna, tomaron la mayor cantidad de UTR zonas *no traducidas* que pudieron encontrar 'ahora mismo', extraída del [gen de la alfa globina](https://www.tandfonline.com/doi/full/10.1080/15476286.2018.1450054). Se sabe que este gen produce de forma robusta muchas proteínas. En años anteriores, los científicos ya habían encontrado formas de optimizar aún más estas zonas *no traducidas* del ARN (según el documento de la OMS), por lo que esta no es exactamente la zona *no traducidas* de alfa globina. Es mejor.

## El péptido señal de la glicoproteína S

Como se señaló, el objetivo de la vacuna es lograr que la célula produzca grandes cantidades de la proteína Spike del SARS-CoV-2. Hasta este punto, hemos encontrado principalmente metadatos y cosas como "convenciones de llamada" en el código fuente de la vacuna. Pero ahora entramos en el territorio real de las proteínas virales.

Sin embargo, todavía nos queda una capa de metadatos. Una vez que el ribosoma (de la espléndida animación anterior) ha producido una proteína, esa proteína todavía necesita ir a alguna parte. Este está codificado en el "péptido señal de la glicoproteína S (secuencia líder extendida)".

La forma de ver esto es que al comienzo de la proteína hay una especie de etiqueta de dirección, codificada como parte de la proteína misma. En este caso específico, el péptido señal dice que esta proteína debe salir de la célula a través del "retículo endoplásmico". ¡Incluso la jerga de Star Trek no es tan elegante como esta!

El "péptido señal" no es muy largo, pero cuando miramos el código, hay diferencias entre el ARN viral y de la vacuna:

(Tenga en cuenta que para fines de comparación, he reemplazado el caracter modificado Ψ por un ARN U normal)

```
           3   3   3   3   3   3   3   3   3   3   3   3   3   3   3   3
Virus:   AUG UUU GUU UUU CUU GUU UUA UUG CCA CUA GUC UCU AGU CAG UGU GUU
Vaccine: AUG UUC GUG UUC CUG GUG CUG CUG CCU CUG GUG UCC AGC CAG UGU GUU
               !   !   !   !   ! ! ! !     !   !   !   !   !            
```

¿Entonces qué está pasando? No he incluido accidentalmente el ARN en grupos de 3 letras. Tres caracteres de ARN forman un codón. Y cada codón codifica un aminoácido específico. El péptido señal de la vacuna consta exactamente de los mismos aminoácidos que el propio virus.

Entonces, ¿cómo es que el ARN es diferente?

Hay 4³ = 64 codones diferentes, ya que hay 4 caracteres de ARN y hay tres de ellos en un codón. Sin embargo, solo hay 20 aminoácidos diferentes. Esto significa que múltiples codones codifican el mismo aminoácido.

La vida utiliza la siguiente tabla casi universal para mapear codones de ARN en aminoácidos:

![](https://berthub.eu/articles/rna-codon-table.png)

*[La tabla de codones de ARN](https://en.wikipedia.org/wiki/DNA_and_RNA_codon_tables) (Wikipedia)*

En esta tabla, podemos ver que las modificaciones en la vacuna (UUU -> UUC) son todas sinónimos. El código de ARN de la vacuna es diferente, pero salen los mismos aminoácidos y la misma proteína.

Si miramos de cerca, vemos que la mayoría de los cambios ocurren en la posición del tercer codón, señalado con un '3' arriba. Y si revisamos la tabla de codones universales, vemos que esta tercera posición de hecho a menudo no importa para qué aminoácido se produce.

Entonces, los cambios son sinónimos, pero ¿por qué están ahí? Mirando de cerca, vemos que todos los cambios excepto uno conducen a más C y Gs.

Entonces, ¿por qué harías eso? Como se señaló anteriormente, nuestro sistema inmunológico ve muy mal el ARN 'exógeno', el código de ARN que proviene del exterior de la célula. Para evadir la detección, la 'U' en el ARN ya fue reemplazada por una Ψ.

Sin embargo, resulta que el ARN con [una mayor cantidad de Gs y Cs](https://www.nature.com/articles/nrd.2017.243) también se [convierte de manera más eficiente en proteínas](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1463026/).

Y esto se ha logrado en el ARN de la vacuna reemplazando muchos caracteres con Gs y Cs siempre que fuera posible.

> Estoy un poco fascinado por el único cambio que no dio lugar a una C o G adicional, la modificación CCA -> CCU. Si alguien sabe la razón, ¡hágamelo saber! Tenga en cuenta que soy consciente de que algunos codones son más comunes que otros en el genoma humano, [pero también leí que esto no influye mucho en la velocidad de traducción](https://journals.plos.org/plosgenetics/article?id%3D10.1371/journal.pgen.1006024).

## La proteína Spike real

Los siguientes 3777 caracteres del ARN de la vacuna tienen un 'codón optimizado' similar para agregar muchas C y G. En aras del espacio, no enumeraré todo el código aquí, pero vamos a acercarnos a un bit excepcionalmente especial. Esta es la parte que lo hace funcionar, la parte que realmente nos ayudará a volver a la vida normal:

```
                  *   *
          L   D   K   V   E   A   E   V   Q   I   D   R   L   I   T   G
Virus:   CUU GAC AAA GUU GAG GCU GAA GUG CAA AUU GAU AGG UUG AUC ACA GGC
Vaccine: CUG GAC CCU CCU GAG GCC GAG GUG CAG AUC GAC AGA CUG AUC ACA GGC
          L   D   P   P   E   A   E   V   Q   I   D   R   L   I   T   G
           !     !!! !!        !   !       !   !   !   ! !              
```

Aquí vemos los cambios habituales de ARN. Por ejemplo, en el primer codón vemos que CUU se cambia a CUG. Esto agrega otra 'G' a la vacuna, que sabemos que ayuda a mejorar la producción de proteínas. Tanto CUU como CUG codifican el aminoácido 'L' o leucina, por lo que nada cambió en la proteína.

Cuando comparamos toda la proteína Spike en la vacuna, todos los cambios son sinónimos... excepto dos, y esto es lo que vemos aquí.

Los codones tercero y cuarto representan cambios reales. Los aminoácidos 'K' y 'V' se reemplazan por 'P' o Prolina. Para 'K' esto requirió tres cambios ('!!!') y para 'V' requirió solo dos ('!!').


**Resulta que estos dos cambios mejoran enormemente la eficacia de la vacuna.**

Entonces, ¿Qué esta pasando aquí? Si observa una partícula real de SARS-CoV-2, puede ver la proteína Spike y, bueno, un montón de picos:

![](https://berthub.eu/articles/sars-em.jpg)

*[Partículas del virus del SARS](https://en.wikipedia.org/wiki/Severe_acute_respiratory_syndrome_coronavirus) (Wikipedia)*

Los picos están montados en el cuerpo del virus ('la proteína de la nucleocápside'). Pero la cuestión es que nuestra vacuna solo genera los picos en sí misma, y no los estamos montando en ningún tipo de cuerpo del virus.

Resulta que, las proteínas Spike independientes y no modificadas colapsan en una estructura diferente. Si se inyecta como vacuna, esto de hecho haría que nuestros cuerpos desarrollen inmunidad ... pero solo contra la proteína de pico colapsada.

Y el verdadero SARS-CoV-2 aparece con el pico puntiagudo. La vacuna no funcionaría muy bien en ese caso.

¿Entonces qué es lo que hay que hacer? En 2017 se describió cómo colocar una doble sustitución de Prolina en el lugar correcto que hacía que las proteínas SARS-CoV-1 y MERS S adoptaran su configuración de 'pre-fusión', incluso sin ser parte del virus completo. Esto funciona porque la prolina es un aminoácido muy rígido. Actúa como una especie de férula, estabilizando la proteína en el estado que necesitamos mostrarle al sistema inmunológico.

La gente que descubrió esto debería estar "chocando los cinco" sin cesar. Debería emanar de ellos cantidades insoportables de presunción. Y todo sería bien merecido.

>¡Actualización! Me ha contactado el laboratorio de McLellan , uno de los grupos detrás del descubrimiento de Proline. Me dicen que lo de "chocarse los cinco" no lo hacen demasiado debido a la pandemia en curso, pero están contentos de haber contribuido a las vacunas. También destacan la importancia de muchos otros grupos, trabajadores y voluntarios.

## El final de la proteína, próximos pasos

Si nos desplazamos por el resto del código fuente, encontramos algunas pequeñas modificaciones al final de la proteína Spike:

```
          V   L   K   G   V   K   L   H   Y   T   s             
Virus:   GUG CUC AAA GGA GUC AAA UUA CAU UAC ACA UAA
Vaccine: GUG CUG AAG GGC GUG AAA CUG CAC UAC ACA UGA UGA 
          V   L   K   G   V   K   L   H   Y   T   s   s          
               !   !   !   !     ! !   !          ! 
```

Al final de una proteína encontramos un codón de "parada", denotado aquí por una "s" minúscula. Esta es una forma educada de decir que la proteína debería terminar aquí. El virus original usa el codón de terminación UAA, la vacuna usa dos codones de terminación UGA, quizás solo por si acaso.

## La región 3' (tres prima) sin traducir

Al igual que el ribosoma necesitaba algo de introducción en el extremo 5', donde encontramos la "región cinco prima no traducida", al final de una región codificante de proteínas encontramos una construcción similar llamada la "región tres prima no traducida".

Se podrían escribir muchas palabras sobre la "región tres prima no traducida", pero aquí cito lo que dice la Wikipedia: “La región tres prima sin traducir juega un papel crucial en la expresión génica al influir en la localización, estabilidad, exportación y eficiencia de traducción de un ARNm. A pesar de nuestra comprensión actual de esta región, sigue siendo relativamente misteriosa".

Lo que sí sabemos es que ciertas "regiones tres primas no traducidas" son muy buenas para promover la expresión de proteínas. Según el documento de la OMS, la "región tres prima no traducida" de la vacuna de BioNTech/Pfizer se seleccionó del “potenciador amino-terminal del ARNm dividido (AES) y el ARN ribosómico 12S codificado mitocondrial para conferir estabilidad al ARN y una alta expresión de proteína total”. A lo que voy, bien hecho.


![](https://berthub.eu/articles/vaccine.jpg)

## El AAAAAAAAAAAAAAAAAAAAAA final de todo

El final del ARNm está poliadenilado. Esta es una forma elegante de decir que termina en una gran cantidad de AAAAAAAAAAAAAAAAAA. Parece que incluso el ARNm está también harto de 2020.

El ARNm se puede reutilizar muchas veces, pero a medida que esto sucede, también pierde algunas de las A al final. Una vez que se agotan las A, el ARNm ya no es funcional y se descarta. De esta manera, la cola 'poli-A' es una protección contra la degradación.

Se han realizado estudios para averiguar cuál es el número óptimo de A al final para las vacunas de ARNm. Leí en la literatura abierta que este alcanzó un máximo de 120 más o menos.

La vacuna BNT162b2 termina con:

```
                                     ****** ****
UAGCAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAGCAUAU GACUAAAAAA AAAAAAAAAA 
AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAAAAAAAA AAAA
```

Se trata de 30 A, luego un "enlazador de 10 nucleótidos" (GCAUAUGACU), seguido de otros 70 A.

Sospecho que lo que vemos aquí es el resultado de una mayor optimización patentada para mejorar aún más la expresión de proteínas.

## Resumiendo

Con esto, ahora conocemos el contenido exacto de ARNm de la vacuna BNT162b2 y, en la mayoría de los casos, entendemos por qué están ahí:

- El CAP para asegurarse de que el ARN se vea como un ARNm normal
- Una región no traducida (UTR) 5 'exitosa y optimizada conocida
- Un péptido señal de codón optimizado para enviar la proteína Spike al lugar correcto (copiado al 100% del virus original)
- Una versión optimizada de codones del pico original, con dos sustituciones de 'Prolina' para asegurarse de que la proteína aparezca en la forma correcta.
- Una región no traducida 3 'exitosa y optimizada conocida
- Una cola poli-A ligeramente misteriosa con un 'enlazador' inexplicable allí

La optimización de codones agrega mucho G y C al ARNm. Mientras tanto, usar Ψ (1-metil-3'-pseudouridililo) en lugar de U ayuda a evadir nuestro sistema inmunológico, por lo que el ARNm permanece el tiempo suficiente para que podamos ayudar a entrenar el sistema inmunológico.

## Más lectura/material audio-visual

En 2017 realicé una presentación de dos horas sobre el ADN, que pueden ver aquí . Al igual que esta página, está dirigida a personas informáticas.

Además, he mantenido una página sobre ['ADN para programadores'](https://k5mdu36cvbs6lbb6orczysify4--berthub-eu.translate.goog/amazing-dna) desde 2001.

También puede disfrutar de [esta introducción a nuestro increíble sistema inmunológico](https://k5mdu36cvbs6lbb6orczysify4--berthub-eu.translate.goog/articles/posts/immune-system/).

Finalmente, [esta lista de las publicaciones de mi blog](https://k5mdu36cvbs6lbb6orczysify4--berthub-eu.translate.goog/articles) tiene bastante material relacionado con el ADN, el SARS-CoV-2 y el COVID.

© 2014-2020 bert hubert


<a name="myfootnote1">1</a>: (NT) Código fuente es un término utilizado en informática para referirse al conjunto de instrucciones que una computadora puede interpretar para llevar a cabo una determinada tarea. En este [enlace](https://github.com/andresmasegosa/Vacunas-Covid/edit/gh-pages/index.md) puedes ver el código fuente de esta web. 

<a name="myfootnote2">2</a>:(NT) Es común [describir](https://blog.powerdata.es/el-valor-de-la-gestion-de-datos/que-son-los-metadatos-y-cual-es-su-utilidad#:~:text=Es%20com%C3%BAn%20describir%20el%20t%C3%A9rmino,la%20informaci%C3%B3n%20de%20los%20mismos.) el término metadatos como datos que describen otros datos o "datos sobre datos". De forma general, en efecto, el concepto de metadatos se refiere a aquellos datos que hablan de los datos, es decir, describen el contenido de los archivos o la información de los mismos.
