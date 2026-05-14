# Unidad 8

## Bitácora de proceso de aprendizaje

https://drive.google.com/drive/folders/1QxrrBv48txxK2hlL_W-ZQ8zLUBNUA19v?usp=drive_link

1. Herramienta elegida con justificación

  - Herramienta: Blender (Geometry Nodes + Shader Editor).
  - Justificación: Se utiliza para procesar datos de audio externos ("LF add",
    "HF multiply") y convertirlos en transformaciones geométricas y cambios de
    color procedurales que superan las capacidades de dibujo 2D.

2. Sistema transferido

  - Sistema: Instanciación rítmica y Campos de Ruido 4D.
  - Descripción: El sistema toma una Grid, distribuye puntos sobre ella y coloca
    instancias de un Mesh Circle. Estas instancias se ven afectadas por una
    Noise Texture 4D y datos de audio para su escala y extrusión.

3. Contexto profesional concreto

  - Contexto: Visualizador de audio (Audio Visualizer) para el track "Capable of
    Love".
  - Uso: Presentación visual para redes sociales o fondo de escenario.

4. Concepto visual

  - Estructuras de Pulso: Una superficie plana que genera protuberancias
    cilíndricas ("Tubos") que crecen en el eje Z. Los colores oscilan entre
    rojo, magenta y azul eléctrico mediante un sombreador de emisión (Emission),
    creando una estética de "placa de datos viva".

5. Explicación de transferencia

  - De p5.js a Blender:
      - El array de partículas de p5.js se transfiere al nodo Distribute Points
        on Faces.
      - La función noise() se transfiere al nodo Noise Texture (4D), donde el
        parámetro W controla la evolución temporal.
      - La reactividad de p5.FFT se gestiona mediante los objetos de entrada LF
        add (Bajos) y HF multiply (Agudos) que multiplican la fuerza de la
        extrusión.


6. Moodboard o referencias
 <img width="1080" height="1020" alt="Diseño sin título" src="https://github.com/user-attachments/assets/11a2a629-b02f-4355-9473-e0d7e9763011" />
Cancion: https://www.youtube.com/watch?v=touqKmxN9n4

8. Pantalla completa o presentación limpia 
<img width="1861" height="950" alt="image" src="https://github.com/user-attachments/assets/2cc0edb3-23d7-49e7-82d9-b4a9e1fcdd46" />


9. Bocetos



10. Mapa de decisiones (Extraído de los Nodos)

**Geometry Node**
<img width="1821" height="743" alt="image" src="https://github.com/user-attachments/assets/3c6cd06d-4f2e-4395-9951-c6f6d5370dec" />

**Shader Editor**
<img width="1554" height="706" alt="image" src="https://github.com/user-attachments/assets/1c5ce964-5d69-4aa0-a4ef-bbf857594868" />

<img width="1293" height="658" alt="image" src="https://github.com/user-attachments/assets/7d3c493b-bd9f-49eb-95ee-2def749ac06f" />



11. Mapa de presentación

  - Bajos (LF): Controlan el nodo LF add, que aumenta la escala general.
  - Agudos (HF): Controlan el nodo HF multiply, que añade detalle al ruido.

12. Uso explícito de IA como materializador

  - Uso técnico: Se utilizó la IA para analizar la jerarquía de los nodos en la
    captura de pantalla y explicar cómo se conectan con los principios de
    movimiento angular y fuerzas de la unidad.


## Bitácora de reflexión
