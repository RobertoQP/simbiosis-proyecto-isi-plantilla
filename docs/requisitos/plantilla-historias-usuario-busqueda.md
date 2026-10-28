# Historias de usuario de búsqueda

| Campo | Valor |
| --- | --- |
| Proyecto | Proyecto Simbiosis |
| Fecha | 10/10/2026 |
| Versión | 1.1 |
| Estado | Borrador (Aprobado para L06) |

## Fuentes comunes

- [Acta de UC-21 Buscar recetas](../captura/acta-acuerdos-uc-21-buscar-recetas.md), v1.1, Vigente.
- [Catálogo de requisitos](catalogo-requisitos.md), UR-08, FR-085, FR-086, FR-087, FR-089, FR-099, FR-102, FR-119, FR-056, FR-066 y FR-192.
- [Modelo de casos de uso](../modelos/modelo-casos-de-uso.md), UC-21.

## Épica de búsqueda

Como **usuario registrado**, quiero **buscar, filtrar y consultar recetas publicadas mediante texto, inclusión o exclusión de ingredientes**, para **localizar de forma rápida y precisa recetas que se adapten a mis preferencias alimentarias y necesidades culinarias.**

**Estado:** descompuesta en US-01, US-02 y US-03. Conserva la necesidad conjunta.

## Justificación de la división

La funcionalidad de búsqueda se ha desglosado en tres historias independientes pero complementarias para permitir una entrega incremental de valor:
1. **US-01** aborda el núcleo de la búsqueda mediante criterios combinados (texto, ingredientes incluidos e ingredientes excluidos) y la gestión de conflictos o validaciones básicas previas.
2. **US-02** se centra en la visualización estructurada de los resultados obtenidos (vistas previas, orden cronológico y manejo de casos sin resultados o sugerencias inteligentes).
3. **US-03** cubre la consulta detallada de la receta seleccionada desde los resultados, permitiendo volver a la lista conservando el estado de la búsqueda.

---

## US-01

**Título:** [Criterios de filtrado y búsqueda combinada]

Como [usuario registrado], quiero [introducir criterios de búsqueda por texto, ingredientes obligatorios e ingredientes a excluir], para [obtener únicamente recetas que se ajusten estrictamente a lo que deseo cocinar o evitar].

**Fuentes:** UR-08; UC-21; FR-085, FR-086, FR-087, FR-089; Acta, apartado 2.

### Criterios de aceptación

- **CA-01.** Dado que el usuario introduce texto selecciona ingredientes para incluir y/o excluir, cuando ejecuta la búsqueda, entonces el sistema valida que indique al menos un criterio (si no hay ninguno, solicita un criterio y no ejecuta la búsqueda) y que no exista el mismo ingrediente en inclusión y exclusión (si hay conflicto, lo señala y bloquea la ejecución hasta su corrección).
- **CA-02.** Dado que la búsqueda incluye texto, cuando se procesa la consulta, entonces el sistema comprueba que la palabra o frase aparezca en el título o descripción breve (respetando orden estricto para frases, sin distinguir mayúsculas ni tildes) combinándose de forma restrictiva con los ingredientes incluidos (deben contenerlos todos) y excluidos (ninguno).
- **CA-03.** Dado que se ejecuta una búsqueda válida, cuando el sistema procesa los datos, entonces considera únicamente recetas publicadas y validadas (omitiendo las pendientes de validación).

---

## US-02

**Título:** [Gestión y visualización de resultados y alternativas]

Como [usuario registrado], quiero [visualizar los resultados ordenados cronológicamente con vistas previas informativas, así como recibir sugerencias o correcciones ortográficas si no hay coincidencias], para [explorar opciones de forma eficiente o reconducir mi búsqueda si cometí un error].

**Fuentes:** UR-08; UC-21; FR-099, FR-102, FR-119; Acta, apartado 2.

### Criterios de aceptación

- **CA-01.** Dado que se obtienen resultados de búsqueda, cuando se muestran en pantalla, entonces se ordenan por fecha de publicación (más recientes primero) y cada vista previa muestra título, imagen (o «Sin imagen»), calificación promedio (o «Sin valoraciones» si carece de ellas) y descripción breve.
- **CA-02.** Dado que una búsqueda no arroja recetas coincidentes, cuando el sistema procesa el vacío, entonces indica que no hay resultados (conservando los criterios) y, si el texto admite corrección ortográfica o alternativas relacionadas (manteniendo las exclusiones intactas), las propone de forma separada sin alterar los criterios ni ejecutar búsquedas automáticas.
- **CA-03.** Dado que ocurre un fallo técnico de consulta, cuando el sistema no puede completarla, entonces informa del problema (sin confundirlo con "sin resultados"), conserva los criterios de la última búsqueda y permite reintentar o terminar.

---

## US-03

**Título:** [Consulta del detalle de la receta y retorno a resultados]

Como [usuario registrado], quiero [seleccionar una receta de los resultados para ver su información completa y poder regresar a la lista sin perder mis filtros], para [revisar los pasos de preparación y decidir si es la receta que buscaba].

**Fuentes:** UR-08; UC-21; FR-056, FR-066, FR-192; Acta, apartado 2.

### Criterios de aceptación

- **CA-01.** Dado que el usuario visualiza los resultados de búsqueda, cuando selecciona una receta de su interés, entonces accede a una vista detallada que muestra título, imagen (si existe), ingredientes, instrucciones de preparación, tiempo de cocción, número de porciones, alias de quien la publicó e insignia de validación.
- **CA-02.** Dado que el usuario se encuentra consultando el detalle de una receta, cuando decide volver atrás, entonces el sistema le retorna a la lista de resultados conservando exactamente los criterios y el orden de la última búsqueda realizada.

---

## Revisión INVEST

| Criterio | Pregunta de revisión | Aplicado | Cumple | Justificación |
| --- | --- | --- | --- | --- |
| Independiente | ¿Puede implementarse cada historia y ofrecer un resultado útil sin implementar antes las otras historias del conjunto? Puede compartir infraestructura con ellas. | Sí | Sí | US-01 genera los datos de consulta, US-02 los muestra y gestiona vacíos, y US-03 consume el detalle. Aunque se complementan, pueden construirse de forma secuencial y modular. |
| Negociable | ¿Cada historia explica qué necesita la persona y deja margen para discutir los detalles de la solución, respetando los acuerdos confirmados? | Sí | Sí | Los criterios definen el "qué" funcional acordado en el acta, dejando abierta la implementación técnica de la interfaz y componentes visuales. |
| Valiosa | ¿La parte «para…» de cada historia explica el beneficio que obtiene la persona al usar esa capacidad? | Sí | Sí | Cada historia aporta un claro beneficio de usabilidad: precisión en el filtrado (US-01), agilidad visual y manejo de errores (US-02), y profundidad de información con retorno de estado (US-03). |
| Estimable | ¿Están claros el alcance y los datos necesarios para estimar el trabajo de cada historia? | Sí | Sí | Las fuentes del catálogo y del acta detallan con precisión los campos de entrada, exclusiones, manejo de nulos y atributos de detalle requeridos. |
| Pequeña | ¿Se centra cada historia en una única capacidad de alcance manejable? | Sí | Sí | Se separan claramente la lógica de filtrado/consulta (US-01), la renderización de listas y ayudas ante vacíos (US-02), y la navegación al detalle (US-03). |
| Comprobable | ¿Los criterios de aceptación de cada historia describen acciones y resultados observables que permiten distinguir un comportamiento correcto de uno incorrecto? | Sí | Sí | Se estructuran mediante formato de comportamiento observable (Dado/Cuando/Entonces), cubriendo restricciones de conflictos, visualizaciones de nulos y persistencia de estados. |

## Dudas pendientes

- Ninguna. Las reglas de negocio, validaciones de conflictos, comportamiento ante ausencia de resultados y la persistencia de los filtros de búsqueda han quedado totalmente aclaradas y aprobadas en el acta de UC-21 para L06.
