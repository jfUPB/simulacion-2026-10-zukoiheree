# Unidad 5
## Bitácora de proceso de aprendizaje

---
**Actividad 1**

1. Capa de comportamiento

Propiedades y clasificación:

- Estado Físico: position (ubicación), velocity (movimiento) y acceleration (cambio de ritmo). Son vectores que definen dónde está y cómo se mueve.

- Estado Vital: lifespan (tiempo de vida). Es una variable numérica que suele empezar en 255.

Condición de "muerte": Una partícula muere cuando su lifespan es menor o igual a 0. La "muerte" técnica (eliminación del array) es instantánea, pero el proceso de morir es gradual, ya que el valor disminuye un poco en cada frame.

Actualización (Motion 101): En cada frame, la partícula sigue la jerarquía:

La aceleración se suma a la velocidad.

La velocidad se suma a la posición.

Extra vital: Se resta un valor constante al lifespan (ej. lifespan -= 2.0).

2. Capa de estructura

Creación: Las partículas las crea el programa principal (el sketch.js), normalmente dentro de la función draw(). En este ejemplo, se añade una partícula nueva en cada frame (particles.push(new Particle())).

Eliminación: El programa principal es quien decide eliminarla. Recorre el array, le pregunta a cada partícula si está muerta (p.isDead()) y, si es así, la borra.

Recorrido inverso (Reverse Loop): Se recorre del final al principio (for (let i = particles.length - 1; i >= 0; i--)) porque al eliminar un elemento con splice(), todos los elementos siguientes se mueven una posición a la izquierda.

¿Qué pasaría si fuera normal? Si borras el elemento [3], el que era [4] ahora es [3]. El bucle pasaría a i = 4, saltándose el elemento que acaba de moverse. El recorrido inverso evita este "salto".

Rendimiento y Memoria: Si no las eliminaras, el array crecería infinitamente.

Experimento: Al comentar el splice(), verás que el contador de partículas sube sin parar. Al llegar a miles, el Frame Rate (FPS) caerá drásticamente porque la computadora tiene que calcular y dibujar objetos que ya ni siquiera son visibles, agotando la memoria RAM y el procesador.

3. Capa de visualización

Elementos visuales: Usa un círculo simple (circle o ellipse) con un borde y un relleno gris.

Conexión Vida-Apariencia: El lifespan está conectado directamente con la opacidad (alpha). El color de relleno se define como fill(127, this.lifespan). Esto hace que la partícula se desvanezca visualmente a medida que se acerca a su eliminación.

Cambio de representación:

Qué cambiarías: Solo el código dentro del método show(). En lugar de circle(), escribirías line(), triangle() o incluso cargarías una imagen con image().

Qué NO cambiarías: No tocarías la lógica de movimiento (update), ni las variables de posición y velocidad, ni el sistema de control del array. La física y la existencia de la partícula son independientes de cómo se ve.

---
**Actividad 2**

1. Comparación con el Ejemplo 4.2

Responsabilidades en Emitter: El nacimiento (addParticle), la actualización masiva (run) y la limpieza de partículas muertas.

Ventaja de encapsular: Permite tener múltiples fuentes de partículas de forma organizada y reutilizar el código fácilmente.

Creadores: El Sketch crea los emisores; cada Emitter crea sus propias partículas.

Jerarquía: Sketch → [Array de Emitters] → [Array de Partículas]. Hay 2 niveles de colección.

2. Transferencia Conceptual (Sin tecnicismos)

"Una entidad central coordina una colección de emisores. Cada emisor gestiona su propia colección de entidades. Estas entidades cambian de estado al recibir una fuerza y desaparecen automáticamente cuando su ciclo de vida termina."

---
**Actividad 3**

Aquí tienes las respuestas breves para la Actividad 03:

1. Comunalidad y Diferencia

En común: Todas comparten las propiedades físicas (posición, velocidad, aceleración) y el ciclo de vida (lifespan).

Diferente: Su apariencia visual (método display) y comportamientos adicionales (como la rotación en los cuadrados).

2. Importancia de la "ignorancia" del Emitter

Es importante porque permite que el sistema sea flexible. El emisor solo sabe que tiene una lista de "partículas" y les ordena ejecutarse; no le importa si son círculos, cuadrados o estrellas. Esto evita tener que escribir un código diferente para cada tipo de objeto.

3. Agregar un tercer tipo

Tendrías que crear: Una nueva clase que extienda de la clase base (ej: class Triangulo extends Particle).

NO tendrías que modificar: Ni la clase Emitter ni la lógica de la clase Particle original. El sistema lo aceptaría automáticamente.

4. Comparación con el Ejemplo 4.2

¿Cambió la lógica? No, la lógica del emisor y la condición de muerte permanecieron idénticas.

Capas: Se modificó la capa de identidad del objeto (variedad de clases), pero la capa estructural (gestión del array y limpieza de memoria) permaneció intacta.

---
**Actividad 4**

## Bitácora de aplicación 

**Actividad 5**

![152060](https://github.com/user-attachments/assets/68ae0ff0-56b8-4409-b24d-d049ce473d6d)

Frases:

"La belleza no es un estado estático, sino un tránsito entre la gravedad que nos siembra y el viento que nos dispersa".

"En la muerte de cada flor, el algoritmo no halla un final, sino la instrucción necesaria para un nuevo comienzo".

---
**Explicación de Decisiones de Diseño**

1. La Semilla

Emisión: Nace de la interacción directa clic o de la muerte de una flor previa. Representa el potencial inicial y la herencia biológica.

Fuerzas: Solo le afecta la gravedad. La decisión busca comunicar "peso" y "destino": la semilla debe caer obligatoriamente para encontrar su propósito en la tierra.

Condición de Muerte: No muere por tiempo, sino por contacto. Al tocar el suelo, su "muerte" es técnica: se elimina para liberar los recursos necesarios para que nazca la flor.

2. La Flor

Visualización: Un tallo vertical que crece y un circulo de color. Es el único elemento anclado y estático, diseñado para ser el pilar visual y el motor del sistema.

Emisión de Pétalos: Es una emisión pulsante y rítmica. Simula la exuberancia de la vida en su punto máximo; la flor "regala" partículas al entorno mientras tiene energía.

Condición de Muerte: Es el elemento con más lifespan. Su muerte activa la regla de supervivencia, disparando nuevas semillas antes de ser borrada de la memoria.

3. El Pétalo

Fuerzas: Ignora la gravedad y responde al Ruido Perlin + Fuerza de Ascenso. La decisión de diseño es crear un "revoloteo" errático que rompa la rigidez matemática y evoque ligereza y libertad.

Visualización: Orientada por su propio vector de velocidad (heading).

Condición de Muerte: Desvanecimiento por transparencia (alpha).

4. Interacción del Usuario

Significado: El usuario actúa como el "Jardinero - Sembrador"

El Clic: Es el acto de creación divina o siembra; el inicio del sistema.

El Movimiento: No es solo estético; al mover el mouse, el usuario altera el vector del viento. Esto significa que el usuario puede elegir entre un jardín en calma o una tormenta que disperse los pétalos violentamente, afectando la estética final de la composición.

5. Gestión de Memoria (splice + loop inverso)

Decisión técnica: Es el "filtro de entropía". En este sistema de crecimiento infinito, el borrado constante es lo que permite que la obra sea eterna. Sin la limpieza de los entes muertos, la vida (el proceso) se detendría por el colapso del hardware.

<img width="941" height="669" alt="image" src="https://github.com/user-attachments/assets/96000b2e-bd6b-4b52-8c0b-fa4067fa767b" />

https://editor.p5js.org/zukoiheree/sketches/2OQsVWh-q

## Bitácora de reflexión

**Actividad 6**

Parte 1

1. Entidad con estado: Cada partícula guarda su propia información (posición, velocidad, aceleración).

2. Ciclo de vida: Tienen un tiempo de existencia limitado (nacen, envejecen y mueren).

3. Colecciones dinámicas: El sistema gestiona listas que crecen y se encogen constantemente.

4. Importancia del borrado: Eliminar partículas muertas es vital para que la memoria no colapse.

5. Separación de lógica: La partícula decide cómo moverse; el sistema decide cuántas hay.

6. Abstracción del emisor: El origen es un objeto independiente que podemos mover o rotar.

7. Sistemas de sistemas: Se pueden crear jerarquías (un grupo de emisores gestionando grupos de partículas).

8. Heterogeneidad: Gracias a la herencia, diferentes tipos de partículas pueden convivir en un mismo sistema.

9. Fuerzas globales y locales: Responden a leyes generales (gravedad) o influencias puntuales (imanes).

10. Independencia visual: El cálculo físico es el mismo, sin importar si la partícula se ve como un punto o una imagen.

Parte 2

1. Lo que se mantiene igual (El Algoritmo)

- La lógica física: El uso de vectores (P,V,A) y las leyes de Newton (F=ma).

- El comportamiento: La regla de que una semilla "muere" para crear una flor, y la flor crea pétalos.

- El ruido: El uso de Ruido Perlin para simular el viento orgánico.

2. Lo que cambiaría (La Técnica)

- Gestión de objetos: En lugar de arreglos y splice(), usarías Object Pooling (reutilizar objetos) o Sistemas de Partículas por GPU para manejar miles de pétalos sin lag.

- Dimensión: Pasarías de coordenadas a un entorno X,Y,Z con luces y materiales 3D realistas.

3. Partes independientes de la herramienta (El Diseño)

- La Narrativa: La idea de "vida, muerte y renacimiento" a través de fuerzas naturales.

- Las Reglas del Mundo: El equilibrio exacto entre la gravedad que tira y el viento que eleva. Eso es diseño algorítmico puro, transportable a cualquier lenguaje.
