# Documentación de Uso de Inteligencia Artificial (AI_USAGE.md)

> **Declaración de Responsabilidad Académica y Técnica:**  
> El equipo de desarrollo de **HexaBlock Suite** asume la responsabilidad total sobre la arquitectura, el diseño de tipos genéricos, la calidad del código, el cumplimiento de los principios SOLID y el funcionamiento de todo el software entregado en este repositorio. Las herramientas de Inteligencia Artificial se utilizaron estrictamente bajo la metodología **SDD (Spec-Driven Development)** como asistentes de productividad y consulta; todo fragmento generado fue auditado, refactorizado y validado mediante pruebas automatizadas por los integrantes del equipo. Ninguna credencial, clave de API ni dato sensible fue compartido en los prompts.

---

## 1. Herramientas de IA Utilizadas

| Nombre de la Herramienta | Versión / Modelo Específico | Rol Asignado dentro del Flujo SDD del Equipo | Integrantes que la Utilizaron |
| :--- | :--- | :--- | :--- |
| **Claude (Anthropic)** | Claude 3.5 Sonnet (`claude-3-5-sonnet-20241022`) | **Architect & Spec Agent:** Refinamiento de contratos genéricos (`HexBoard<TCell>`, `IGameRules`), verificación de restricciones de tipo (*bounded generics*), generación de borradores PlantUML y validación de aislamiento de capas Clean Architecture. | Dev 1, Dev 2, Dev 3 |
| **Cline / Continue.dev (VS Code API)** | Claude 3.5 Sonnet / DeepSeek-V3 / GPT-4o-mini | **Core Implementer & Tester Agent:** Asistencia en la implementación de algoritmos sobre coordenadas axiales `(q, r)`, generación de casos base de pruebas unitarias bajo patrón AAA y estructuración de decoradores AOP. | Dev 1, Dev 2, Dev 3 |
| **GitHub Copilot** | Copilot Pro (Student Pack - GPT-4o engine) | **In-line Autocomplete:** Autocompletado de código repetitivo (Mappers, DTOs, serialización JSON de niveles, definiciones Swagger/OpenAPI y constructores). | Dev 1, Dev 2, Dev 3 |

---

## 2. Registro de Uso por Tarea (Bitácora de Ingeniería)

### Entrada #01 — Diseño del Tablero Hexagonal Genérico y Recorrido Axial
* **Fecha / Commit Asociado:** `2026-10-28` | Commit: `feat(engine): add generic HexBoard with axial coordinates`
* **Tarea o Problema Abordado:** Modelar la estructura de datos `HexBoard<TCell>` en coordenadas axiales `(q, r)` capaz de soportar tableros de radio variable (radio 2 y radio 4) y máscaras de celdas arbitrarias (para Hexa Fit), junto con un iterador genérico por ejes y anillos.
* **Herramienta y Modelo Utilizado:** Claude 3.5 Sonnet (Rol: *Architect & Core Agent*).
* **Prompt Proporcionado (Transcripción / Paráfrasis Fiel):**
  > *"Actúa como Arquitecto en Clean Architecture. Necesitamos implementar en la Capa de Dominio la clase genérica `HexBoard<TCell>` usando coordenadas axiales `HexCoord(q, r)` basadas en la referencia de Red Blob Games. Restricciones obligatorias: 1) Cero dependencias de librerías de UI o colecciones externas; 2) Debe aceptar la forma del tablero mediante inyección en el constructor para soportar tanto hexágonos regulares de radio R como figuras irregulares cerradas; 3) Implementa el patrón Iterator para recorrer los 3 ejes (q constante, r constante, s = -q-r constante) y las 6 direcciones de vecindad; 4) Incluye un método de rotación de piezas en múltiplos de 60 grados usando coordenadas cúbicas implícitas."*
* **Resultado Obtenido:** La IA generó la estructura base de `HexCoord`, la clase `HexBoard<TCell>` utilizando un mapa hash interno indexado por coordenadas axiales, las fórmulas matemáticas de rotación de 60°: $(q, r, s) \rightarrow (-r, -s, -q)$ y la implementación inicial de `AxialAxisIterator`.
* **Modificaciones Realizadas por el Equipo:**
  1. Se corrigió el método de validación de límites en figuras arbitrarias (Hexa Fit), ya que la IA asumía inicialmente una distancia radial uniforme $\max(|q|, |r|, |q+r|) \le \text{radius}$. Se extrajo una interfaz `IBoardShapeStrategy` para cumplir con el principio Open/Closed (OCP).
  2. Se añadió inmutabilidad estricta a `HexCoord` y se implementó el patrón *Prototype* (`clone()`) sobre el estado del tablero para soportar la función de deshacer (`UndoMoveUseCase`) sin mutar referencias activas.
* **Lecciones Aprendidas y Limitaciones Identificadas:** Los modelos de lenguaje manejan con alta precisión las identidades algebraicas de grillas hexagonales ($\Delta q + \Delta r + \Delta s = 0$), pero tienden a acoplar la forma geométrica del tablero dentro de la misma clase si no se les exige explícitamente delegar la topología en una estrategia separada.

---

### Entrada #02 — Algoritmo de Transferencia en Cascada (Hexa Sort)
* **Fecha / Commit Asociado:** `2026-11-05` | Commit: `feat(hexa-sort): implement cascade transfer rule`
* **Tarea o Problema Abordado:** Implementar la regla de resolución de `HexaSortRules` (implementación de `IGameRules<ColorStack, PlaceStackMove, SortGameState>`) donde al colocar una pila, las fichas superiores del mismo color en celdas adyacentes se transfieren recursivamente hacia la pila con mayor cantidad de dicho color y se eliminan al llegar a 10 fichas.
* **Herramienta y Modelo Utilizado:** Claude 3.5 Sonnet vía Continue.dev (Rol: *Core Implementer Agent*).
* **Prompt Proporcionado (Transcripción / Paráfrasis Fiel):**
  > *"Implementa el método `resolve(board: HexBoard<ColorStack>, lastMove: PlaceStackMove)` dentro de `HexaSortRules` cumpliendo el contrato `IGameRules`. Reglas de negocio: al colocar una pila en `lastMove.coord`, inspecciona los 6 vecinos axiales. Si el color en la cima (`topColor`) de uno o más vecinos coincide con el `topColor` de la celda actual, transfiere las fichas contiguas de ese color hacia la pila que tenga mayor cantidad acumulada de ese color en su cima. Si una pila alcanza >= 10 fichas del mismo color consecutivo, elimínalas, suma una meta cumplida y emite el evento `StackMerged`. El proceso debe repetirse en cascada (BFS/cola de prioridad) hasta alcanzar un estado estable."*
* **Resultado Obtenido:** La IA entregó un algoritmo basado en una cola FIFO que evaluaba las celdas modificadas, realizaba el `pop/push` entre pilas de colores y disparaba la limpieza al contar 10 elementos iguales.
* **Modificaciones Realizadas por el Equipo:**
  1. **Corrección de bucle infinito:** Cuando dos pilas adyacentes tenían exactamente la misma cantidad de fichas del mismo color en la cima, el código de la IA alternaba la transferencia entre ambas indefinidamente. El equipo definió una regla de desempate determinista basada en el orden lexicográfico de `HexCoord(q, r)` y priorizó como receptora la celda recién colocada por el jugador.
  2. Se desacopló la emisión de eventos de animación devolviendo una lista ordenada de pasos de resolución (`List<ResolutionStep>`) para que la Capa de Presentación reproduzca la cascada visualmente sin bloquear el hilo principal ni contaminar el dominio con temporizadores (`async/await` de UI).
* **Lecciones Aprendidas y Limitaciones Identificadas:** La IA suele resolver reglas de juegos en cascada de forma síncrona e instantánea o introduciendo `delays` asíncronos dentro del dominio. Fue fundamental intervenir manualmente para mantener la pureza síncrona de la Capa 1 (Dominio) devolviendo pasos discretos de transición de estado.

---

### Entrada #03 — Implementación de Aspectos AOP mediante Decoradores SOLID
* **Fecha / Commit Asociado:** `2026-11-12` | Commit: `feat(application): add logging and profiling AOP decorators for generic use cases`
* **Tarea o Problema Abordado:** Implementar Programación Orientada a Aspectos (AOP) para Logging/Trazabilidad y Métricas de Rendimiento (Profiling) sobre los casos de uso genéricos sin usar librerías externas de AOP.
* **Herramienta y Modelo Utilizado:** Claude 3.5 Sonnet (Rol: *Architect Agent*).
* **Prompt Proporcionado (Transcripción / Paráfrasis Fiel):**
  > *"Diseña una interfaz base `IUseCase<TRequest, TResponse>` en la Capa 2 (Aplicación) y dos aspectos transversales usando el patrón Decorator y el principio DIP de SOLID (sin librerías AOP ni reflexión): 1) `LoggingUseCaseDecorator<TRequest, TResponse>` que intercepte `execute()`, registre el estado de entrada, salida y errores a través de un puerto `ILoggerPort`; 2) `PerformanceProfilingDecorator<TRequest, TResponse>` que mida en milisegundos el tiempo de ejecución de operaciones como la cascada de Hexa Sort y emita una alerta si supera un umbral configurable."*
* **Resultado Obtenido:** Código completo de la interfaz `IUseCase`, ambos decoradores genéricos componibles en cadena y la demostración de cómo envolver `ApplyMoveUseCase<TCell, TMove, TState>` en el contenedor de inyección de dependencias.
* **Modificaciones Realizadas por el Equipo:**
  1. Se ajustó la serialización del log en `LoggingUseCaseDecorator` para enmascarar tokens JWT cuando envuelve casos de uso de autenticación (`SyncLeaderboardUseCase`), previniendo fugas de datos sensibles en consola.
  2. Se inyectó una abstracción `IClockPort` en lugar de usar el reloj estático del sistema dentro de `PerformanceProfilingDecorator` para poder simular tiempos de ejecución exactos en las pruebas unitarias.
* **Lecciones Aprendidas y Limitaciones Identificadas:** La combinación de tipos genéricos con el patrón *Decorator* permitió cumplir el requisito de AOP de forma 100% portable y tipada; sin embargo, fue necesario reemplazar el reloj del sistema sugerido por la IA por una interfaz inyectable para no romper el determinismo de los tests.

---

### Entrada #04 — Generación de Suite de Pruebas Unitarias con Test Doubles Genéricos
* **Fecha / Commit Asociado:** `2026-11-18` | Commit: `test(engine): add unit tests for GameEngine with fake cell type`
* **Tarea o Problema Abordado:** Verificar que `GameEngine<TCell, TMove, TState>` funciona en aislamiento total de las 4 variantes utilizando un tipo de celda falso (`FakeCell`) y cumpliendo la nomenclatura `should_[resultado]_when_[condicion]` y el patrón AAA.
* **Herramienta y Modelo Utilizado:** DeepSeek-V3 / GPT-4o-mini vía Cline (Rol: *Tester Agent*).
* **Prompt Proporcionado (Transcripción / Paráfrasis Fiel):**
  > *"Actúa como Tester Agent. A partir del contrato `GameEngine<TCell, TMove, TState>` y `IGameRules<TCell, TMove, TState>`, genera una suite de pruebas unitarias usando un test double `FakeCell`, `FakeMove` y `FakeState`. Usa estrictamente el patrón AAA (con comentarios `// Arrange`, `// Act`, `// Assert`) y nombra cada test con la convención `should_[expected_result]_when_[condition]`. Cubre: inicialización de partida, rechazo de movimiento inválido, aplicación y resolución exitosa, notificación a observadores y transición a estado GameOver."*
* **Resultado Obtenido:** Suite de 12 pruebas unitarias estructuradas bajo AAA con clases stub para las reglas, política de puntuación y fuente de piezas.
* **Modificaciones Realizadas por el Equipo:**
  1. Se eliminaron 2 pruebas redundantes que verificaban getters internos del motor y se agregaron pruebas específicas para verificar las restricciones de tipo acotado (`TCell : IMergeable<TCell>`) y la reproducibilidad exacta al inyectar la misma semilla en `IPieceSource`.
* **Lecciones Aprendidas y Limitaciones Identificadas:** El uso de un System Prompt estricto para el *Tester Agent* ahorró más del 60% del tiempo de escritura de boilerplate en mocks, permitiendo al equipo concentrarse en diseñar casos borde complejos para las 4 variantes.

---

## 3. Evaluación Crítica del Uso de IA

### 3.1 Porcentaje Estimado de Asistencia de IA
A nivel global del repositorio, aproximadamente un **35% – 40% del código base** contó con generación inicial o autocompletado asistido por IA, distribuido de forma asimétrica según la criticidad arquitectónica de cada capa:
* **Capa 1 — Dominio y Motor Genérico (~20% IA / 80% Equipo):** El diseño de los parámetros de tipo, restricciones genéricas y separación de responsabilidades fue realizado íntegramente por el equipo; la IA solo asistió en fórmulas de geometría axial e iteradores base.
* **Capa 2 — Casos de Uso y Aspectos AOP (~30% IA / 70% Equipo):** La estructura de los decoradores SOLID fue acelerada con IA y refinada manualmente para garantizar pureza arquitectónica.
* **Capa 3 y 4 — Adaptadores, DTOs, Mappers y UI/REST (~50% IA / 50% Equipo):** Mayor aprovechamiento del autocompletado para serialización JSON de niveles, esquemas OpenAPI/Swagger y vistas.
* **Suites de Pruebas Automatizadas (~55% IA / 45% Equipo):** El andamiaje AAA y los *test doubles* fueron generados con el *Tester Agent* a partir de casos de prueba diseñados y auditados por el equipo.

### 3.2 Casos de Resultados Incorrectos o Subóptimos (Alucinaciones y Deuda Técnica) y su Corrección
1. **Violación sutil del Principio de Sustitución de Liskov (LSP) en `UndoMove`:** En una primera iteración, la IA sugirió incluir el método `undo()` directamente dentro de `IGameRules<TCell, TMove, TState>` haciendo que `HexaLinesRules` y `HexaSortRules` lanzaran un `UnsupportedOperationException`. **Detección y Corrección:** Durante la revisión de arquitectura del equipo, se detectó que esto violaba tanto LSP como ISP (Sección 3.2). Se rechazó el cambio y se segregó la capacidad en la interfaz independiente `IUndoable`, implementada únicamente en Hexa Fit (ilimitado) y Hexa Merge (1 movimiento) mediante el patrón *Command*.
2. **Deslizamiento en Ejes Diagonales en Hexa Merge:** Al generar la lógica de deslizamiento en las 6 direcciones para Hexa Merge, la IA confundió el orden de recorrido de las celdas al deslizar hacia el eje Noroeste ($\Delta q = -1, \Delta r = 0$), procesando las fichas desde el centro hacia el borde en lugar de desde el borde receptor hacia atrás, lo que provocaba fusiones dobles en un solo turno (ej. `2 + 2 -> 4` y en el mismo deslizamiento `4 + 4 -> 8`). **Detección y Corrección:** El error fue capturado inmediatamente por la prueba unitaria `should_merge_only_once_per_turn_when_three_identical_tiles_collide_on_northwest_axis`. Se invirtió manualmente el sentido del `AxialAxisIterator` según el vector de dirección del movimiento (`fix(hexa-merge): correct slide direction on north-west axis`).
3. **Acoplamiento de Estado en el Backend REST:** En la generación inicial de la entidad de progreso del backend, la IA creó tablas separadas por cada juego. **Detección y Corrección:** El equipo reestructuró el modelo de dominio y los endpoints para parametrizar todo mediante la dimensión obligatoria `gameId` con un esquema JSON polimórfico único, asegurando que agregar una quinta variante no requiera migraciones de base de datos ni nuevos controladores.

### 3.3 Reflexión sobre el Impacto en la Productividad y la Calidad del Software
La adopción de una metodología **Spec-Driven Development (SDD)** transformó la IA de un generador impredecible de código en un multiplicador disciplinado de ingeniería:
* **Impacto en la Productividad:** Redujo drásticamente el tiempo invertido en tareas mecánicas (mapeadores de datos, configuración de CI/CD en GitHub Actions, documentación Swagger y escritura de pruebas repetitivas), permitiendo dedicar el esfuerzo intelectual del equipo a perfeccionar el polimorfismo paramétrico del motor y diseñar manualmente los 15 niveles de Hexa Fit y 10 niveles de Hexa Sort.
* **Impacto en la Calidad:** Exigir que toda interacción con IA partiera de un contrato de interfaz previo y terminara obligatoriamente en una prueba unitaria bajo el patrón AAA evitó el fenómeno de "dependencia ciega". Cada integrante del equipo comprende a fondo el flujo de ejecución desde la vista hasta el motor genérico y está en capacidad de demostrar en vivo cómo extender la suite con una quinta variante sin modificar una sola línea de `GameEngine`.