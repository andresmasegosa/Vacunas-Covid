# Ingeniería inversa del código fuente de la vacuna BioNTech / Pfizer SARS-CoV-2

> Este artículo es una traducción del siguiente artículo publicado originalmente en inglés: [Reverse Engineering the source code of the BioNTech/Pfizer SARS-CoV-2 Vaccine](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). La traducción está hecha con ayuda de [Google Translate](https://translate.google.es/). Si ves una errata, eres bienvenid@ a hacer una PR. 
> 
> El artículo aquí descrito proporciona descripción de la vacuna de BioNTech/Pfizer utilizando una analogía con las computadoras. Creo que es una perspectiva muy interesante que ayuda a comprender la complejidad y la potencia de este tipo de tecnología. 


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




<a name="myfootnote1">1</a>: Código fuente es un término utilizado en informática para referirse al conjunto de instrucciones que una computadora puede interpretar para llevar a cabo una determinada tarea. En este [enlace](https://github.com/andresmasegosa/Vacunas-Covid/edit/gh-pages/index.md) puedes ver el código fuente de esta web. 
