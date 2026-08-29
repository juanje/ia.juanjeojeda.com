---
title: "La inteligencia está más allá del LLM"
description: "Si lo que hace inteligente a un sistema es su capacidad de adaptar su conducta, entonces un LLM por sí solo no es especialmente inteligente. Lo que le da flexibilidad es todo lo que ocurre a su alrededor."
date: 2026-08-14
tags:
  - agentes-ia
  - sistemas-complejos
  - memoria
  - harness
  - emergencia
aliases:
  - inteligencia-mas-alla-del-llm
---

*Si lo que hace inteligente a un sistema es su capacidad de adaptar su conducta, entonces un LLM por sí solo no es especialmente inteligente. Lo que le da flexibilidad es todo lo que ocurre a su alrededor.*

> Also available in [English](https://ai.juanjeojeda.com/intelligence-is-beyond-the-llm).

---

Se suele confundir lo que es un LLM con un chatbot (ChatGPT, Claude, Gemini…), pero una cosa es el modelo y otra el software que lo envuelve y permite que sea más útil al usuario.

Si llamas directamente al modelo a través de su API y le haces varias veces la misma pregunta, incluso si le dices o preguntas otras cosas en medio, te responderá algo muy parecido cada vez. En cambio, si lo haces en un chatbot, la respuesta irá cambiando. Sobre todo si entre una vez y la siguiente le das más contexto sobre tu pregunta o le corriges algo.

La razón es que el LLM no conserva memoria entre una llamada y la siguiente. Cada vez que le preguntas parte de cero, no te conoce ni sabe qué le has dicho antes. Pero un chatbot va guardando la conversación y se la vuelve a pasar en cada llamada, para que el modelo parta con una «memoria» temporal de lo que han hablado y parezca que continúa por donde lo dejaron.

Esto funciona hasta cierto punto. Esa «memoria temporal» (la llamada *ventana de contexto*) tiene un límite. Y las conversaciones se guardan por sesiones o chats. Por eso, cuando abres un chat nuevo, el modelo no «recuerda» nada ni sigue por donde lo dejaron. Algunos chatbots ya están creando memorias de más «largo plazo» para guardar cosas sobre ti entre conversaciones, pero eso también ocurre en la capa de software, no en el modelo.

Así que esa sensación de que el chatbot se adapta a ti, de que puede seguir el hilo o incluso cambiar de opinión, emerge de la interacción del LLM con esa capa de software, con esos «recuerdos» guardados y con la conversación previa que se le va pasando. No es algo que haga el modelo por sí solo.

Y aquí es donde se pone interesante.

> La neurocientífica [Suzana Herculano-Houzel](https://www.suzanaherculanohouzel.com/) define la inteligencia como la **flexibilidad conductual**, es decir, la capacidad de un organismo de modificar su comportamiento basándose en experiencias pasadas pero también en la **anticipación de estados futuros** que tengan valor para el individuo.

Esa definición cambia la pregunta. Si lo que hace inteligente a un sistema no es su potencia bruta sino su capacidad de adaptar su conducta, entonces un LLM por sí solo no es especialmente inteligente. Siempre responde desde el mismo estado. Lo que le da flexibilidad es todo lo que ocurre a su alrededor.

## El modelo no está solo

Lo que hacen los chatbots es solo el principio. Alrededor del modelo puedes añadir mucho más que historial de conversación: memoria persistente, herramientas, identidad, bucles de feedback y un entorno de trabajo. Eso es lo que llamamos el _arnés_ (_harness_).
El arnés es lo que convierte al modelo en parte de un agente (ya sea un asistente de código, una automatización para marketing o lo que sea).

> Un agente es un proceso que vive en un bucle, que tiene un objetivo, una forma de obtener información de su entorno, tiene herramientas para interactuar con él y reflexiona sobre los resultados obtenidos. El arnés es lo que le da esas herramientas, [[agente-ia-pudiera-aprender|memoria]] y conexión con el entorno al agente.

Un LLM aislado responde a partir de sus pesos y del contexto que le metes en cada llamada. Un agente puede conservar información entre sesiones, consultar documentos, actuar sobre el entorno, observar el resultado y volver a intentarlo. Esa secuencia cambia su estado y modifica lo que puede hacer después. No es que el modelo se vuelva más inteligente. Es que el sistema completo desarrolla capacidades que no existían en el modelo aislado.

## La memoria no tiene que estar dentro

Tendemos a pensar en la memoria como algo que vive dentro de un cerebro, o dentro de un modelo. Pero no siempre funciona así.

Fíjate en el moho mucilaginoso (*slime mold*). Un organismo sin cerebro ni sistema nervioso que resuelve laberintos. ¿Cómo? Deja rastros químicos en el entorno que modifican su propio comportamiento posterior. Las huellas cambian el paisaje de decisión, no el organismo. El sistema usa esas marcas para evitar caminos ya explorados y conservar los más útiles. La memoria está distribuida entre el organismo y el entorno.

En un agente de IA pasa algo parecido. La memoria puede estar en el historial de la conversación, en un fichero, en el resultado de una herramienta o en una regla que el sistema ha consolidado después de varias experiencias. No tiene que estar dentro del LLM para modificar su comportamiento. Los ficheros modifican el contexto, no el modelo, exactamente como las huellas químicas del moho modifican el paisaje, no las células.

Yo lo he comprobado construyendo el sistema de memoria para [Buddy](https://github.com/juanje/buddy), un agente que llevo desarrollando como proyecto open source desde hace meses. Sin memoria, el agente solo reacciona al presente inmediato. Con memoria puede comparar lo que pasa ahora con lo que pasó antes, ajustar su comportamiento y acumular una historia que le cambia las opciones disponibles. La diferencia no es de potencia. Es de contexto acumulado.

## La trayectoria crea estado

En sistemas complejos, la historia importa. Dos sistemas que empiezan siendo casi iguales pueden divergir si experimentan una secuencia diferente de interacciones.

Piensa en dos agentes idénticos que reciben la misma pregunta. Uno acaba de leer un documento técnico y de recibir una corrección del usuario. El otro arranca en frío. La respuesta será distinta, y no solo por el no determinismo del modelo, sino porque cada uno ha construido un contexto diferente. La trayectoria ha creado un estado distinto, y el estado modifica el espacio de respuestas posibles.

Yo lo veo con [Buddy](https://github.com/juanje/buddy). Tengo dos instancias: una para trabajo y otra para temas personales. Empezaron con la misma estructura, pero después de semanas de uso han desarrollado memorias, reglas y prioridades diferentes. No porque yo lo planificara, sino porque cada una ha vivido una historia distinta. La divergencia no es un error. Es el sistema adaptándose a su contexto de uso.

## Contexto como paisaje de posibilidades

Solemos tratar el contexto como un paquete de información que le entregamos al modelo: cuanta más información, mejor respuesta. Pero el contexto funciona también como restricción: hace que unas posibilidades sean más visibles y otras queden fuera del foco.

Esta diferencia se nota al comparar cómo se puede organizar el conocimiento de un agente. Puedes darle fragmentos sueltos recuperados por similitud (lo que hace un RAG convencional), y el agente recibe piezas potencialmente útiles pero desconectadas de su contexto original. Es como si alguien te subrayara párrafos de libros distintos y te los pusiera delante sin su contexto original.

Lo que yo uso en Buddy funciona de otra forma. El agente empieza con un índice ligero, sigue un puntero, lee un documento completo y continúa navegando por sus conexiones cuando lo necesita. Como empezar en una página de Wikipedia y seguir sus enlaces. El camino recorrido ayuda a construir el significado, porque la arquitectura del contexto determina qué conceptos aparecen juntos y qué relaciones puede detectar el agente.

Diseñar el contexto no es solo elegir qué información darle. Es influir en el paisaje de posibilidades desde el que responde.

## Diseñar las condiciones, no el resultado

Si la capacidad emerge del sistema completo, construir agentes no consiste únicamente en elegir un modelo más potente.

En mi experiencia, lo que más ha cambiado la utilidad de mi agente no han sido las actualizaciones del LLM. Ha sido decidir:
- Qué memoria conserva el sistema y cuál descarta.
- Cómo accede a esa memoria (navegación, no volcado).
- Qué herramientas amplían su espacio de acción.
- Qué feedback recibe después de actuar.
- Qué restricciones orientan su trayectoria.
- Qué tareas deterministas se pueden hacer por código, en vez de _rezar_ para que el LLM se acuerde de hacerlas.

Cuando el agente navega por sus ficheros, no está leyendo un almacén pasivo. Los está usando como parte de su proceso de pensamiento. El contexto externo modifica qué puede conectar, qué puede recordar y qué acciones puede considerar. El agente no piensa solo con el modelo. Piensa con el modelo, los ficheros, las herramientas y el entorno en el que opera.

Hay que tener en cuenta que esto no garantiza un resultado concreto. Estamos hablando de sistemas complejos, no de recetas. Pero sí cambia el espacio de lo posible. Y eso, en la práctica, se nota.

La próxima vez que evalúes un agente, no mires solo qué modelo utiliza. Pregúntate qué recuerda, por dónde navega, qué herramientas puede usar y cómo cambia después de interactuar con el mundo.

La pregunta no es únicamente cómo hacer un modelo más inteligente. Es cómo diseñar un sistema que le permita desarrollar una conducta más flexible.
