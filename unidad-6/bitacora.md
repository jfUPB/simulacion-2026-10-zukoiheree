# Unidad 6

## Bitácora de proceso de aprendizaje

**Actividad 2**

1. ¿Qué es un Agente Autónomo?

Es una entidad que tiene intención. A diferencia de un objeto pasivo (como una pelota), un agente observa su entorno y toma sus propias decisiones para cumplir un objetivo. No necesita que alguien lo empuje; él "decide" hacia dónde moverse.

2. ¿Qué es una Steering Force (Fuerza de Dirección)?

Es una fuerza calculada para corregir el camino de un agente. Se basa en la fórmula de Craig Reynolds:

**Fuerza de Direccion = Velocidad Deseada = Velocidad Actual**

Es el "timonazo" que da el agente para pasar de su movimiento actual a la trayectoria que realmente quiere seguir.

3. Steering Force vs. Gravedad

- Fuerza Externa (Gravedad): Es pasiva y universal. Le "sucede" al objeto sin que este pueda evitarlo. No hay deseo, solo reacción física.

- Steering Force: Es activa e interna. Representa la voluntad del agente. Es la respuesta inteligente del ser a lo que está viendo o sintiendo.

4. ¿Por qué es útil para el Comportamiento Visual?

Porque nos permite diseñar personalidad y narrativa en lugar de solo física:

Un agente puede verse "nervioso" si su fuerza de dirección es errática.

Puede verse "valiente" si ignora obstáculos para llegar a un punto.

Esto genera comportamientos emergentes: patrones complejos (como una bandada de pájaros o un cazador persiguiendo una presa) que cuentan una historia visual sin necesidad de animar cada paso manualmente.

**Actividad 3**

**Análisis Técnico**

1.  Construcción: Se basa en una **rejilla (grid)** que cubre el lienzo. Normalmente se usa **Ruido Perlin 2D** para asignar a cada celda un ángulo que cambie suavemente respecto a sus vecinas.
2.  Representación: Cada celda contiene un **vector de dirección**. Es como una señal de tráfico invisible que dice: "si pasas por aquí, debes ir hacia allá".
3.  Consulta del Agente: El agente toma su posición $(x, y)$, la divide por la resolución de la rejilla (ej. cada 20px) y encuentra los índices de la celda correspondiente en el mapa.
4.  Decisión de Movimiento: El vector de la celda se convierte en la **Velocidad Deseada**. El agente aplica la fórmula de Reynolds: `Fuerza = Deseada - Actual`. El agente no cambia su dirección instantáneamente, sino que "gira" hacia ella.

### Parámetros Críticos

*   Resolución: Determina qué tan "detallado" es el campo. Celdas pequeñas = curvas cerradas; celdas grandes = flujos rectos.
*   MaxSpeed: La velocidad máxima del agente.
*   MaxForce: La capacidad de giro. Si es baja, el agente es "terco" y le cuesta seguir el flujo (derrapa).
*   **Cantidad de Agentes:** Define la densidad visual y la saturación de la obra.

### Modificación y Efecto
*   Modificación: Reducir drásticamente el `maxForce` (ej. a 0.01).
*   Efecto Visual: Los agentes adquieren una gran **inercia**. No pueden seguir las curvas del campo y "derrapan", creando trayectorias mucho más amplias y suaves que ignoran los detalles finos del ruido.

### Análisis Narrativo y Estético

1.  Tipo de Movimiento: Es un movimiento **laminar y coordinado**. Se siente como una voluntad colectiva guiada por una mano invisible.
2.  Sensaciones: Evoca el flujo del agua en un río, bancos de peces, el viento moviendo la hierba o corrientes magnéticas. Transmite orden dentro del caos.
3.  Pieza Musical:Funcionaría perfecto con **minimalismo ambiental** (como Brian Eno) o un solo de **piano melancólico** (estilo Erik Satie), donde las notas fluyen de forma continua y sin sobresaltos.

**Actividad 4**

1. Las Tres Reglas Básicas

*   Separación: Es la regla de "espacio personal". Evita que los agentes choquen entre sí. Cada uno mira a sus vecinos más cercanos y empuja en dirección opuesta para no amontonarse.
*   Alineación: Es la regla de "seguir la corriente". Cada agente observa hacia dónde van sus vecinos y trata de apuntar su velocidad en esa misma dirección, logrando que el grupo se mueva como una unidad.
*   Cohesión: Es la regla de "quedarse juntos". Cada agente calcula dónde está el centro del grupo cercano y camina hacia ese punto para evitar que la bandada se fragmente y se pierdan individuos.

2. Parámetros de Control
El sistema se controla principalmente mediante **pesos (weights)** y **radios de visión**:
*   Pesos: Multiplicadores que deciden qué regla es más importante (ej. `sepWeight`, `aliWeight`).
*   Distancia de vecindad: Define qué tan lejos alcanza a "ver" un agente para considerar a otro como su vecino.

3. Modificación de Pesos y Efecto
*   Modificación: Aumentar al triple la **Separación** y reducir a cero la **Cohesión**.
*   Efecto: El grupo "explota" visualmente. Los agentes huyen unos de otros con agresividad. Se pierde la idea de colectivo y el sistema se convierte en una nube de partículas individuales que evitan el contacto a toda costa.

4. Comportamiento Emergente Observado

*   Compacto: Cuando la *Cohesión* es alta, forman grupos densos.
*   Disperso: Cuando la *Separación* domina.
*   Estable: Hay un equilibrio; el grupo se mantiene unido pero con aire entre agentes.
*   Nervioso: Si el `maxForce` es muy alto, los agentes vibran al intentar corregir su rumbo.
*   Caótico: Si no hay *Alineación*, cada uno tira para su lado aunque estén cerca.
*   Fluido: El estado ideal de Reynolds; parece un río de seres vivos sorteando obstáculos con elegancia.

5. Análisis Narrativo

*   Atmósfera Visual: Produce una sensación de **inteligencia colectiva** y vida biológica. Es hipnótico porque el patrón global es complejo y siempre cambiante, aunque las reglas individuales sean simples. Sugiere armonía y supervivencia.
*   Relación Musical: Funcionaría increíble con **Jazz experimental** (donde hay improvisación individual pero dentro de una estructura común) o con **música electrónica rítmica (Techno)**, donde el movimiento de los agentes podría sincronizarse con los pulsos y las frecuencias del bajo.

**Actividad 5**

Comparativa: Flow Fields vs. Flocking

| Aspecto | Flow Fields (Campos de Flujo) | Flocking (Sistemas de Bandada) |
| :--- | :--- | :--- |
| **Tipo de Movimiento** | **Laminar y guiado.** Los agentes siguen caminos predefinidos por el entorno (como un río). | **Social y reactivo.** Los agentes se mueven basándose en la posición de sus vecinos. |
| **Nivel de Control Visual** | **Alto.** El diseñador controla la "carretera" por la que van los agentes modificando el ruido o la rejilla. | **Medio/Bajo.** El diseñador dicta las reglas, pero el grupo decide su trayectoria final de forma autónoma. |
| **Nivel de Emergencia** | **Bajo.** El patrón global es el campo mismo. Una partícula no afecta el comportamiento de otra. | **Muy Alto.** El comportamiento del grupo surge de interacciones locales; es difícil predecir la forma final. |
| **Atmósfera / Sensación** | Orden, hipnosis, estructura invisible, corrientes naturales, calma técnica. | Vida biológica, instinto, comunidad, agitación, sincronía orgánica. |
| **Relación Musical** | Ideal para texturas sonoras constantes, pads de sintetizador o drones. | Ideal para ritmos percusivos, improvisaciones y cambios dinámicos. |
| **Ventajas** | Muy eficiente (puedes tener miles de agentes). Fácil de predecir visualmente. | Altamente expresivo y "vivo". Crea patrones visuales fascinantes y complejos. |
| **Limitaciones** | Puede volverse monótono o rígido si el campo no cambia con el tiempo. | Computacionalmente costoso (cada agente debe "mirar" a todos los demás). |

Visuales según el estado de ánimo

Si tuviera que diseñar los visuales para distintos tipos de canciones, esta sería mi elección algorítmica:

1.  Canción Contemplativa: Flow Fields
    *   *Por qué:* Usaría un campo de ruido Perlin muy suave y lento. La sensación de que los elementos se desplazan por un orden invisible invita a la reflexión y a la paz, sin movimientos bruscos que distraigan.
2.  Canción Agresiva: Flocking (con pesos extremos)
    *   *Por qué:* Usaría Flocking con una fuerza de **Separación** muy alta y un `maxForce` elevado. Esto crearía agentes que "chocan" y huyen violentamente unos de otros, generando un caos nervioso que visualiza la tensión y la fricción de la música.
3.  Canción Melancólica: Flow Fields (con baja densidad)
    *   *Por qué:* Pocos agentes moviéndose en direcciones divergentes y descendentes. El Flow Field permite crear la sensación de "deriva", donde cada elemento parece estar solo siguiendo su propio camino hacia el olvido, sin interactuar con los demás.
4.  Canción Eufórica: Flocking (con alta Cohesión y Alineación)
    *   *Por qué:* Ver a cientos de agentes unirse de repente en un solo remolino perfectamente sincronizado comunica una explosión de energía colectiva. La **Cohesión** hace que el visual "explote" y se "reúna" al ritmo de los clímax musicales.

## Bitácora de aplicación 

**Actividad 6**





## Bitácora de reflexión

