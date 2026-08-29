---
title: "¿Y si tu agente de IA pudiera aprender de verdad?"
description: "Sobre cómo dar memoria, identidad y la capacidad de olvidar a los agentes locales."
date: 2026-04-10
tags:
  - agentes-ia
  - memoria
  - memoria-basada-en-ficheros
  - progressive-disclosure
  - aprendizaje
  - agentic-buddy
aliases:
  - que-pasaria-si-tu-agente-pudiera-aprender
---

*Sobre cómo dar memoria, identidad y la capacidad de olvidar a los agentes locales.*

> Also available in [English](https://ai.juanjeojeda.com/what-if-your-ai-agent-could-actually-learn).

---

Llevo años investigando cómo aprendemos. No en un laboratorio, sino más bien en una inmersión lenta y personal en neurociencia, psicología cognitiva, [teoría de sistemas complejos](https://blog.juanjeojeda.com/) y cómo todo eso conecta con cosas prácticas como la adquisición de habilidades, el cambio de hábitos o el desarrollo de la experticia. Es una obsesión genuina. Y durante el último año, conforme empecé a trabajar más en serio con agentes de IA (usándolos a diario para el trabajo o construyendo proyectos experimentales como [macsdk](https://github.com/juanje/macsdk), [Mystery agents](https://github.com/juanje/mystery-agents) y otros), había una pregunta que no dejaba de rondarme: **¿podría aplicarse alguna de estas ideas sobre aprendizaje humano para hacer mejores agentes?**

No mejores en el sentido de «modelos más listos». Eso es problema de Anthropic y OpenAI. Lo digo en un sentido más fundamental: **¿podríamos hacer agentes que aprendan de verdad a partir de la experiencia?** Que acumulen conocimiento útil como hacemos nosotros, quedándose con lo que importa, conectando ideas relacionadas, soltando lo que ya no es relevante y desarrollando nuevas habilidades con el tiempo.

Llevo un tiempo explorando esto, construyendo un sistema open source llamado [Agentic Buddy](https://github.com/juanjeojeda/agentic-buddy), un asistente personal de propósito general con memoria persistente basada en ficheros. Vive en su propio repositorio (directorio) y me ayuda con todo tipo de cosas: gestión de tareas, investigación, escritura, seguimiento de proyectos personales. Es mi campo de pruebas para entender qué funciona y qué no cuando intentas que un agente aprenda, y para desarrollar principios que luego puedan aplicarse a otros tipos de agentes.

Quiero compartir lo que he ido encontrando, porque creo que la generación actual de agentes de IA, tan capaces como amnésicos e incapaces de aprender nada nuevo entre sesiones, puede llevarse mucho más lejos de lo que la mayoría imagina.

### El colega brillante con amnesia

Si has usado un agente basado en LLMs para programar, investigar, escribir o simplemente pensar en un problema, conoces el patrón. Empiezas una conversación, cargas algo de contexto, haces tu trabajo, cierras la sesión. Mañana empiezas desde cero.

Es como tener un colega brillante con amnesia. Cada mañana entras y se le ha olvidado todo. Tu proyecto. Tus decisiones. El error que habíais acordado evitar. Desaparecido.

Las herramientas más cuidadas te dejan escribir un fichero de instrucciones estático que se carga al inicio de cada sesión. Ayuda. Pero sigue siendo un documento fijo que mantienes a mano. No aprende. No crece. No se adapta.

El verdadero cuello de botella no es la capacidad del modelo, los modelos actuales son muy buenos. El cuello de botella es el **contexto**: el agente no sabe qué importa _ahora mismo_, qué se decidió ya, qué sigue yendo mal o qué aprendiste la semana pasada.

El problema es que, aunque las ventanas de contexto sigan creciendo, más contexto no significa mejor contexto. Un agente con 200K tokens de historial en bruto es peor que uno con 2K tokens de la información correcta en el momento correcto.

Lo que necesitas no es una memoria más grande. Es una memoria más inteligente.

### Los ficheros como sustrato de la memoria

La primera idea es casi vergonzosamente simple: **usar el sistema de ficheros como cerebro del agente.**

Nada de bases de datos. Nada de _vector stores_. Nada de _embeddings_. Nada de APIs externas. Solo ficheros [Markdown](https://es.wikipedia.org/wiki/Markdown) en un repositorio Git.

Y cuando digo _solo_ ficheros, lo digo en serio. Toda la implementación es un conjunto de directorios y documentos Markdown, no hay código. Apuntas un asistente de código como [Claude Code](https://claude.ai/code/family) o [Cursor](https://cursor.com/) a ese directorio, y los ficheros de instrucciones convierten al asistente en un agente con memoria persistente que aprende con el tiempo. La «magia» está toda en cómo se estructuran los ficheros y cómo se instruye al agente a usarlos.

Esto funciona porque los asistentes de código actuales son, en el fondo, agentes muy buenos leyendo y escribiendo ficheros de texto. Esa es su capacidad nativa. Y resulta que leer y escribir ficheros es todo lo que necesitas para crear memorias, desarrollar habilidades nuevas y consultar conocimiento previamente aprendido. El sistema de ficheros se convierte en el cerebro del agente y las capacidades de manipulación de ficheros que ya tiene el agente se convierten en el mecanismo de aprendizaje.

¿Por qué ficheros? Porque son legibles tanto por humanos como por agentes. Son versionables. Git te da todo el historial gratis. Son portables, sin servidor, sin migraciones, sin _vendor lock-in_. Y son componibles: puedes enlazarlos, dividirlos, fusionarlos y reorganizarlos según el sistema evoluciona.

[Andrej Karpathy](https://karpathy.ai/) [propuso hace poco algo en esta línea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): sustituir el [RAG](https://es.wikipedia.org/wiki/Generaci%C3%B3n_aumentada_por_recuperaci%C3%B3n) tradicional por una wiki Markdown mantenida por el propio LLM, señalando que la parte tediosa de mantener una base de conocimiento es el trabajo administrativo y los LLMs llevan ese trabajo sin despeinarse. Es una observación aguda.

Pero creo que la idea se puede llevar más lejos. Una wiki de conocimiento es una pieza del puzzle. ¿Y si el agente tuviera también _memoria_, un sentido estructurado de qué ha pasado, qué se decidió, qué patrones se repiten? ¿Y si pudiera _aprender de su propia experiencia_, curando activamente su conocimiento, promoviendo lo útil y soltando lo que no lo es?

### Progressive disclosure: la clave que lo hace funcionar

Si volcaras todo el conocimiento del agente en su ventana de contexto al inicio de cada sesión, el sistema colapsaría. Las ventanas de contexto son finitas y caras. Pero, sobre todo, la mayor parte de ese conocimiento sencillamente no es relevante para lo que estás haciendo en ese momento.

La **_progressive disclosure_** (revelación progresiva) resuelve esto. El agente empieza cada sesión cargando un único fichero ligero (~100 líneas). Ese fichero contiene lo justo para orientarlo: quién eres (brevemente), qué está pasando ahora mismo y, lo más importante, **punteros a donde vive todo lo demás.** El agente no carga todo. Sabe _dónde mirar_ para encontrar todo.

Durante la conversación, cuando aparece un tema que requiere conocimiento más profundo, el agente sigue el puntero correspondiente y lee ese fichero concreto. Cada fichero de conocimiento contiene a su vez enlaces a otros ficheros relacionados, de forma que el agente puede ir profundizando conforme lo necesite. Piénsalo como navegar por una wiki bien organizada: empiezas en el índice, vas a la sección que necesitas y desde ahí sigues las referencias cruzadas. Nunca tienes que leerlo todo.

Aquí es donde el sistema se separa del RAG tradicional. En una pipeline RAG, el sistema codifica tu consulta, busca en una base de datos vectorial y recupera los trozos de texto más similares, fragmentos que muchas veces vienen despojados de su contexto original. El modelo tiene que reconstruir el sentido a partir de piezas sueltas. Con _progressive disclosure_, cuando el agente lee un fichero, obtiene un **documento coherente con su contexto**: resumen, puntos clave, ejemplos y enlaces a conceptos relacionados. Y el agente ha llegado a ese fichero a través de una cadena de enlaces con contexto, así que ya tiene el marco que explica _por qué_ esa información es relevante.

Para conocimiento curado a escala moderada (decenas o bajas centenas de ficheros), esta navegación estructurada me da resultados que me sorprenden una y otra vez, a menudo mejores de lo que esperaría de un sistema de recuperación mucho más pesado, precisamente porque el contexto se preserva y es coherente.

### Cuatro tipos de memoria

No toda la información es igual. Una decisión tomada ayer, un concepto aprendido el mes pasado y un borrador en el que estás trabajando ahora mismo cumplen funciones muy distintas y requieren reglas de ciclo de vida muy distintas.

La arquitectura usa cuatro zonas de memoria, cada una con una función propia:

```
 ┌───────────────────────────────────────────────────────┐
 │                  MEMORIA DEL AGENTE                   │
 │                                                       │
 │  ┌──────────────┐  Siempre cargada. ~100 líneas.      │
 │  │   Memoria    │  Estado actual + punteros al resto. │
 │  │  de trabajo  │  La atención activa del agente.     │
 │  └──────┬──▲────┘                                     │
 │   carga │  │ promover / degradar                      │
 │  bajo   │  │ según uso                                │
 │  demanda│  │                                          │
 │  ┌──────▼──┴────┐                                     │
 │  │   Memoria    │  Conceptos, habilidades, proyectos. │
 │  │  semántica   │  Conocimiento a largo plazo.        │
 │  │              │  Hebbiana: usado → se queda,        │
 │  │              │            sin uso → archivado.     │
 │  └──────▲───────┘                                     │
 │         │ destilar                                    │
 │  ┌──────┴───────┐                                     │
 │  │   Memoria    │  Logs diarios.                      │
 │  │   episódica  │  Decisiones, contexto, lecciones.   │
 │  │              │  Crudo → estructurado → destilado.  │
 │  └──────────────┘                                     │
 ├───────────────────────────────────────────────────────┤
 │  ┌──────────────┐                                     │
 │  │    Mente     │  Espacio de trabajo del usuario.    │
 │  │  extendida   │  Borradores, listas, documentos.    │
 │  │              │  El agente ayuda; el usuario manda. │
 │  └──────────────┘                                     │
 └───────────────────────────────────────────────────────┘
```

La **memoria de trabajo** es lo único que se carga automáticamente: el fichero ligero con quien eres, qué está pasando ahora y los punteros al conocimiento más profundo. También es donde vive la **identidad** del agente: dos ficheros cargados al inicio de cada sesión que le dan al agente consistencia entre interacciones.

Uno define el carácter del agente (SOUL.md): cómo debe comportarse, qué valora, cómo se comunica, en qué debe plantarse. Es más una especificación de personalidad que da forma a sus juicios, no solo a sus respuestas. El otro (USER.md) captura quién eres tú: tu rol, tus preferencias, tu contexto actual. El agente va actualizando este fichero orgánicamente conforme aprende sobre ti, marcando lo que ha inferido por su cuenta frente a lo que le has dicho directamente, para que puedas corregirlo. Este patrón de identidad basado en ficheros fue popularizado por [OpenClaw](https://docs.openclaw.ai/concepts/agent#bootstrap-files-injected), y está ganando tracción porque es transparente, editable y versionable. Sin esto, el agente puede recordar hechos, pero no se siente consistente: servicial un día, genérico al siguiente.

La **memoria semántica** es el conocimiento a largo plazo: conceptos que el agente ha aprendido, patrones que ha notado, proyectos que está siguiendo, habilidades que ha desarrollado. Cada fichero registra cuándo se accedió por última vez y con qué frecuencia, metadatos que deciden qué se promueve más cerca de la memoria de trabajo y qué se archiva.

La **memoria episódica** es el registro en bruto de lo que pasó, logs diarios de conversaciones que se destilan hacia la memoria semántica durante los ciclos de mantenimiento.

La **mente extendida** es el espacio de trabajo del usuario. Documentos, borradores, tableros, cosas que el agente puede leer y en las que puede contribuir, pero que nunca reorganiza por su cuenta.

La analogía con la memoria humana es deliberada y resulta que es más que una metáfora. Las distintas reglas de ciclo de vida para cada zona (memoria de trabajo refrescada a diario, conocimiento semántico curado por uso, logs episódicos compactados con el tiempo, espacio del usuario intocado) reflejan cómo nuestros propios sistemas de memoria manejan distintos tipos de información. Es una heurística de diseño realmente útil.

### Aprender a través de ciclos de mantenimiento

Esta es la parte que lleva del sistema de «memoria» al «aprendizaje».

El agente aprende a través de una cadena de ciclos de mantenimiento cada vez más profundos:

```
 Conversaciones
       │
       ▼
  ┌──────────┐    /reflect
  │ Memoria  │◄───────────────  Extraer decisiones, lecciones,
  │episódica │                  observaciones al log diario
  └────┬─────┘
       │  /daily
       │  Consolidar: convertir observaciones
       │  repetidas en conocimiento
       ▼
  ┌───────────┐
  │  Memoria  │  /weekly
  │ semántica │──────────────►  Calibrar: ¿están los ficheros
  └────┬──────┘                 adecuados en memoria de trabajo?
       │                        Generalizar patrones.
       │  promover relevantes
       ▼                        /monthly
  ┌──────────┐                ┌────────────────────────────┐
  │ Memoria  │◄───────────────│ Archivar conocimiento sin  │
  │de trabajo│                │ uso. Detectar contradic-   │
  └──────────┘                │ ciones. Podar habilidades  │
                              │ obsoletas.                 │
                              └────────────────────────────┘
```

**_Reflect_** procesa la conversación actual, extrayendo decisiones, tareas, ideas y lecciones al log del día. También detecta _observaciones_: patrones que podrían convertirse en conocimiento. Piénsalo como llevar un diario.

La **consolidación diaria** revisa los logs del día, crea nuevos conceptos a partir de patrones repetidos, forma enlaces entre conocimiento existente y actualiza la memoria de trabajo con lo que es relevante en ese momento.

La **revisión semanal** da un paso atrás: qué se ha hecho, qué está atascado, ¿están los ficheros correctos en memoria de trabajo? Calibra, generaliza patrones y detecta problemas estructurales.

El **mantenimiento mensual** hace el trabajo profundo: archivar logs antiguos, podar conocimiento al que no se ha accedido, detectar contradicciones, revisar la estructura general.

Cada ciclo opera a una escala de tiempo distinta y cada uno se construye sobre el anterior. El mismo principio que la repetición espaciada en el aprendizaje: revisiones frecuentes y superficiales capturan lo importante rápido; revisiones poco frecuentes y profundas capturan patrones estructurales que solo se ven con el tiempo.

**Los ciclos de mantenimiento no son solo limpieza. Son donde ocurre el aprendizaje de verdad.** El ciclo diario convierte observaciones en conocimiento. El semanal generaliza. El mensual poda y detecta contradicciones. Sin ellos, solo tienes una pila creciente de notas. Con ellos, tienes una base de conocimiento que se cura a sí misma activamente.

En mi implementación actual, estos ciclos se disparan manualmente (comandos _slash_ en el editor). Es una decisión deliberada: quiero que el sistema funcione con distintos agentes locales y editores, y la mayoría todavía no soportan _hooks_ de automatización fiables. Pero no hay nada fundamental en el disparo manual. Estos ciclos están diseñados para automatizarse, y cuando corren por su cuenta la memoria se vuelve mucho más potente: el agente se mantiene solo, y tú solo lo usas.

### Del ruido al conocimiento

Hay un mecanismo concreto que merece la pena destacar. El agente lleva un diario de observaciones, cosas que ha notado pero sobre las que no ha actuado. _«El usuario siempre revisa el estado de CI lo primero.»_ _«Este patrón de error no para de aparecer en el servicio de autenticación.»_

Una observación aislada es ruido. Cuando la misma observación aparece dos veces, eso es una señal. El ciclo de mantenimiento puede entonces convertirla en una **habilidad** (_un procedimiento reutilizable_), una **regla** (_una pauta de comportamiento_) o un **concepto** (_una pieza de conocimiento_).

El aprendizaje es conservador por diseño. Nada de sacar conclusiones a partir de un solo dato. La excepción son las correcciones directas: si le dices al agente «no hagas eso», propone una regla al momento, porque **tu feedback directo es la señal más fuerte que existe**.

### Olvidar no es un bug, es una característica

Hay algo que me costó un tiempo apreciar: **el olvido controlado no es una limitación. Es lo que permite que el sistema escale.**

Cada fichero de conocimiento lleva metadatos:

```yaml
---
last_accessed: 2026-04-09
access_count: 12
created: 2026-03-15
---
```

Los ficheros consultados con frecuencia se promueven a la memoria de trabajo. Los ficheros sin tocar durante más de 30 días y con pocos accesos se archivan durante el mantenimiento mensual.

Esto está directamente inspirado en la plasticidad hebbiana en neurociencia: «las neuronas que se activan juntas se conectan juntas». Los ficheros que se _usan_ se quedan en primer plano. Los que no, se desvanecen. La importancia no se declara, emerge del uso.

Y el mecanismo hebbiano funciona en ambas direcciones. Los ciclos de mantenimiento no solo archivan ficheros sin uso, sino que también **promueven el conocimiento usado con frecuencia más cerca del contexto inicial del agente.** Los ficheros que el agente consulta a menudo pasan a estar referenciados directamente desde el fichero de memoria de trabajo, quedando a un paso en vez de a dos o tres. La estructura del sistema se adapta de forma natural para que el conocimiento más útil sea el más accesible.

Sin esto, la base de conocimiento crece sin límite, la relación señal/ruido se degrada y el agente gasta contexto en información que dejó de ser relevante hace semanas. El olvido es lo que mantiene el sistema útil a lo largo del tiempo.

(Los ficheros archivados no se borran. Se mueven a un directorio de archivos, y Git preserva todo. Es más como guardar cajas en el desván que tirarlas.)

### Más allá del asistente personal

Agentic Buddy es un asistente personal, mi campo de pruebas para entender estos principios en la práctica. Pero los principios en sí son generales. Lo que me importa no es la herramienta concreta que he construido, sino las ideas que hay detrás: memoria estructurada basada en ficheros, _progressive disclosure_, ciclos de mantenimiento que convierten la experiencia en conocimiento, ficheros de identidad para la consistencia y olvido controlado. Son patrones de diseño y pueden aplicarse a tipos muy distintos de agentes.

Algunos ejemplos de hacia dónde creo que podría ir esto:

- **Conocimiento de equipo y onboarding.** Un agente que aprende cómo trabaja un equipo concreto a partir de interacciones continuas, no de una wiki estática. Observa decisiones recurrentes, patrones comunes, convenciones establecidas. Cuando un miembro nuevo del equipo pregunta _«¿por qué elegimos este enfoque?»_, el agente lo sabe porque estaba ahí cuando se tomaron esas decisiones y sus ciclos de mantenimiento han destilado los patrones importantes del ruido.
- **Aprendizaje de infraestructura e incidentes.** Una base de conocimiento viva de patrones de fallo y sus resoluciones, que evoluciona con la propia infraestructura. Cuando aparece un problema similar, el agente tiene contexto de qué funcionó la última vez, qué no, y qué ha cambiado desde entonces.
- **Conocimiento del código.** Un agente que aprende un código base concreto, no solo el código, sino el razonamiento detrás de él. ¿Por qué se estructuró este módulo así? ¿Qué _trade-offs_ se consideraron? Este tipo de conocimiento tácito suele vivir solo en la cabeza de las personas y se pierde cuando dejan el equipo.

Son solo puntos de partida. En todos los casos, aplican los mismos patrones de diseño.

### Por qué creo que esto importa

Estamos en un punto en el que los agentes de IA son útiles, pero están fundamentalmente limitados por su falta de contexto persistente. Los modelos son capaces. Lo que falta es la infraestructura de memoria alrededor de ellos.

Los enfoques más comunes para resolver esto son o bien «meterlo todo en una ventana de contexto gigante» (caro, ruidoso, no escala) o bien «montar una pipeline de RAG compleja» (potente, pero pesada y opaca). Creo que hay un camino intermedio, **ficheros estructurados con _progressive disclosure_ y gestión del ciclo de vida**, que es más simple, más transparente y suficientemente bueno para un rango sorprendente de casos de uso.

Pero el punto más importante es este: conforme los agentes de IA se vuelvan parte rutinaria de cómo trabajamos, la pregunta de _qué recuerdan, cómo aprenden y cómo olvidan_ va a convertirse en un problema de ingeniería de primer orden. No solo para las empresas que construyen productos de IA, sino para cualquier equipo que use un asistente de IA. Los agentes que conozcan tu contexto, los patrones de tu equipo, la historia de tu código base y los modos de fallo de tu infraestructura serán mucho más útiles que los que empiezan de cero en cada sesión.

Si quieres experimentar con estas ideas, [Agentic Buddy](https://github.com/juanjeojeda/agentic-buddy) es open source y se pone en marcha en unos cinco minutos. Es un asistente personal, no un plugin para proyectos de código, pero los patrones que aprendes usándolo pueden informar cómo construirías memoria en otros tipos de agentes.

Lo que me llevo de todo esto es que la inteligencia de la IA actual no vive solo en los modelos. Un agente, como cualquier agente complejo en un entorno complejo, puede desarrollar memoria y habilidades a partir de reglas sorprendentemente simples y una forma básica de interactuar con su entorno. En este caso, el sistema de ficheros.

Tendemos a complicarnos la vida cuando tratamos de diseñar el sistema perfecto de entrada. Yo hice exactamente eso al principio: sobrediseñé la estructura de ficheros que pensaba que el agente iba a necesitar, escribí reglas estrictas para cada caso límite y traté de anticipar cada escenario. No funcionó muy bien. Lo que acabó funcionando fue lo contrario: una estructura mínima, menos reglas y más simple, y dejar que el sistema organizara su propio «cerebro» conforme el uso lo demandaba. La estructura que emergió del uso real resultó ser más eficiente y más útil que cualquier cosa que yo hubiera diseñado sobre el papel.

Esa es la parte que no esperaba. Entré pensando que estaba diseñando un sistema de memoria. Acabé viendo cómo crecía uno.
