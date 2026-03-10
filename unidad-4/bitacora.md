# Unidad 4

## Bitácora de proceso de aprendizaje

**Actividad 2**

- ¿Qué está pasando y cuál es la interacción?

En esta simulación vemos un objeto (una barra con dos círculos en los extremos, similar a una batuta) que gira alrededor del centro de la pantalla.

Lo que sucede: Se está aplicando el marco Motion 101 pero de forma angular. En lugar de mover el objeto por el espacio (x,y), se está cambiando su orientación (ángulo) con una aceleración y velocidad constantes.

Interacción: En este código específico, la aceleración es fija (0.001), por lo que la batuta gira cada vez más rápido de forma automática. El usuario puede observar cómo la inercia angular aumenta la velocidad de giro.

- ¿Por qué trasladar el origen (translate) al centro?

Por defecto, el punto (0,0) de p5.js está en la esquina superior izquierda. Si rotamos ahí, el objeto giraría alrededor de esa esquina y desaparecería de la pantalla.

Razón: Se hace para que el eje de rotación coincida con el centro de la pantalla. Al mover el sistema de coordenadas, permitimos que el objeto pivote sobre su propio centro de forma simétrica.

- Relación entre el sistema de coordenadas y rotate()
  
Esta es la clave de la transformación en p5.js: rotate() no gira el objeto, gira todo el sistema de coordenadas (el lienzo/rejilla).
Imagina que el lienzo es una hoja de papel milimetrado. Al llamar a rotate(), estás clavando un alfiler en el (0,0) y girando toda la hoja. Todo lo que dibujes después quedará inclinado según el ángulo actual de la "hoja".

- ¿Por qué se dibuja en (0, 0)? 

Porque después de hacer el translate, el centro de la pantalla es el nuevo (0,0). Dibujar la línea de -50 a 50 centrada en el origen es mucho más intuitivo matemáticamente que calcular senos y cosenos manualmente para cada punto.

- ¿Por qué rotan si el código es el mismo? 

Aunque en cada frame el código dice "dibuja un círculo en (50, 0)", la variable angle aumenta en cada frame. Como el sistema de coordenadas (la rejilla) está girado un poco más en cada cuadro, el punto (50, 0) se desplaza visualmente en un círculo, aunque para la computadora siempre se esté dibujando en el mismo lugar de su rejilla rotada.

- Identificación del marco Motion 101

Aceleración Angular: aAcceleration = 0.001; (La fuerza que cambia el ritmo de giro).
Velocidad Angular: aVelocity += aAcceleration; (La rapidez con la que cambia el ángulo).
Posición Angular (Ángulo): angle += aVelocity; (La orientación actual del objeto).

- ¿Qué hace la función heading()?

La función heading() calcula el ángulo de dirección de un vector.

Matemáticamente: Utiliza la función arcotangente (atan2(y, x)) para decirnos hacia dónde apunta la flecha del vector.

Si tu vector de velocidad apunta hacia la derecha y un poco hacia abajo, heading() te devolverá el ángulo exacto (en radianes) necesario para que p5.js sepa "mirar" en esa dirección.

- ¿Qué hacen push() y pop()?

push(): Guarda la configuración actual (dónde está el origen (0,0), qué color de relleno hay, qué rotación existe).

pop(): Borra todos los cambios hechos desde el último push() y devuelve todo a como estaba antes.

- ¿Qué hace rectMode(CENTER)?

Por defecto (CORNER): rect(0, 0, 30, 10) dibuja el rectángulo empezando desde su esquina superior izquierda. Al rotar, el rectángulo giraría como una puerta desde su bisagra.

CENTER: Las coordenadas se refieren al centro exacto del rectángulo.


- Relación entre el ángulo de rotación y el vector de velocidad
- 
La relación es de alineación narrativa. Queremos que la "cara" del objeto siempre mire hacia donde se está moviendo.

**Actividad 3**

<img width="632" height="516" alt="image" src="https://github.com/user-attachments/assets/89413daa-2669-46e4-8068-4af69e0e8f9c" />
https://editor.p5js.org/zukoiheree/sketches/qsw2vAP3k

**Actividad 4**

Aquí tienes el resumen clave de las modificaciones:

1. Motion 101 con Fuerzas

Modificación: En applyForce(f) usa acceleration.add(f) y al final de update() usa acceleration.mult(0).

Razón: Las fuerzas son instantáneas. Si no reseteas la aceleración en cada frame, los empujes se acumulan eternamente y el objeto alcanza velocidades infinitas.

2. Color del Attractor

Busca el método show() o display() dentro de la clase Attractor y cambia el valor de fill().

Ejemplo: fill(255, 100, 0); (Naranja).

3. Implementación de Interacción

Para que dragging y rollover funcionen, debes añadir esta lógica:

- Rollover (Encima): En el draw, calcula la distancia:
- 
this.rollover = dist(mouseX, mouseY, this.position.x, this.position.y) < this.mass;

Dragging (Arrastrar):

- En mousePressed(): si el mouse está sobre el atractor, this.dragging = true.
- En mouseReleased(): this.dragging = false.
- En update(): si dragging es cierto, iguala this.position a la del mouse.



## Bitácora de aplicación 



## Bitácora de reflexión
