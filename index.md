# Ingeniería inversa del código fuente de la vacuna BioNTech / Pfizer SARS-CoV-2

> Este artículo es una traducción de un artículo publicado en inglés [Reverse Engineering the source code of the BioNTech/Pfizer SARS-CoV-2 Vaccine](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). LA traducción está hecha con ayuda de [Google Translage](https://translate.google.es/). Si ves una errata, eres bienvenid@ de hacer una PR. 
> 
> El artículo aquí descrito proporciona descripción de la vacuna de BioNTech/Pfizer utilizando una analogía con las computadoras. Creo que es una perspectiva muy interesante que ayuda a comprender la complejidad y la potencia de este tipo de tecnología. 



¡Bienvenidos! En esta publicación, analizaremos, carácter por carácter, el código fuente<sup>[1](#myfootnote1)</sup> de la vacuna de ARNm del SARS-CoV-2 de BioNTech / Pfizer.

>Quiero agradecer al gran número de personas que dedicaron tiempo a la  de este artículo por su legibilidad y corrección. Sin embargo, todos los errores siguen siendo míos, pero me encantaría escucharlos rápidamente en bert@hubertnet.nl o @PowerDNS_Bert

Esto que digo puede resultar algo discordants: la vacuna es un líquido que se inyecta en el brazo. ¿Cómo podemos hablar entonces de código fuente?


Esta es una buena pregunta, así que comencemos con una pequeña parte del mismo código fuente de la vacuna BioNTech / Pfizer, también conocida como [BNT162b2](https://en.wikipedia.org/wiki/Tozinameran) , también conocida como Tozinameran también conocida como [Comirnaty](https://twitter.com/PowerDNS_Bert/status/1342109138965422083).

![Primeros 500 caracteres del ARNm de BNT162b2. Fuente: [Organización Mundial de la Salud](https://mednet-communities.net/inn/db/media/docs/11889.doc).](https://berthub.eu/articles/bnt162b2.png)


El "corazón" de la vacuna de ARNm BNT162b tiene el código digital que aparece en esta imagen de arriba. En total, tiene 4284 caracteres, por lo que cabría en unos cuantos tweets. Al comienzo del proceso de producción de la vacuna, alguien cargó este código en una impresora de ADN (sí), que luego convirtió estos bytes almacenenados en su disco duro en moléculas de ADN reales.


![Una impresora de ADN [Codex DNA](https://codexdna.com/products/bioxp-system/) BioXp 3200](https://berthub.eu/articles/bioxp-3200.jpg)


<a name="myfootnote1">1</a>: Código fuente es un término utilizado en informática para referirse al conjunto de instrucciones que una computadora puede interpretar para llevar a cabo una determinada tarea. En este [enlace](https://github.com/andresmasegosa/Vacunas-Covid/edit/gh-pages/index.md) puedes ver el código fuente de esta web. 
