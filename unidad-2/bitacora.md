# Unidad 2

## Bitácora de proceso de aprendizaje

**Actividad 2**

Usar vectores en lugar de variables separadas (xspeed, yspeed) tiene tres ventajas clave:

- Simplicidad (Menos variables): En lugar de gestionar x, y, z, xspeed, yspeed, zspeed por separado, solo tienes dos objetos: position y velocity. El código es más limpio.

- Abstracción Matemática: Puedes aplicar operaciones complejas (como calcular la distancia, el ángulo o la fuerza de atracción) con una sola línea de código: v1.dist(v2) o v1.mag().

- Escalabilidad: Si pasas de 2D a 3D, no tienes que crear nuevas variables ni reescribir tus funciones; el vector simplemente añade una componente .z y el resto del código funciona igual.

---

**Actividad 3**

La lógica matemática es la misma, pero la estructura del código cambia en tres puntos clave:

- Sustitución de variables: Eliminé this.x y this.y y las reemplacé por una única variable this.pos (un objeto vector).

- Inicialización: En el constructor, en lugar de asignar números, usé createVector(width/2, height/2).

- Actualización de posición: En lugar de hacer this.x += stepX, ahora creo un pequeño vector de "paso" y lo sumo al vector de posición usando el método .add().

Esto permite que el movimiento se trate como una entidad física completa.

```java
code
JavaScript
download
content_copy
expand_less
let walker;

function setup() {
  createCanvas(400, 400);
  // Inicializamos el caminante
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    // 1. Convertimos x e y en un vector de posición
    this.pos = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    // Accedemos a las componentes x e y del vector
    point(this.pos.x, this.pos.y);
  }

  step() {
    // 2. Creamos un vector para el paso aleatorio
    // Esto genera un movimiento en x:(-1, 0, 1) e y:(-1, 0, 1)
    let step = createVector(random(-1, 1), random(-1, 1));
    
    // 3. Sumamos el vector de paso a la posición actual
    this.pos.add(step);
  }
}
```

Aunque para un punto simple parezca más trabajo, ahora podrías cambiar fácilmente step por una fuerza (como el viento) o una aceleración simplemente sumando otro vector, sin tener que escribir fórmulas matemáticas manuales para cada eje.

---

**Actividad 4**

1. ¿Qué resultado esperas obtener en el programa anterior?

Se espera que el primer console.log muestre los valores iniciales del vector (6, 9) y que el segundo muestre los valores modificados (20, 30), demostrando que la función alteró el vector original.

2. ¿Qué resultado obtuviste?

```
"p5.Vector Object [6, 9, 0]" (o similar, dependiendo de la versión).

"p5.Vector Object [20, 30, 0]"

"Only once" (proveniente del draw).
```

3. ¿Qué tipo de paso se está realizando en el código?

En JavaScript, las variables primitivas (números, strings, booleanos) se pasan por valor (se crea una copia local), pero los objetos (como los vectores, arrays o diccionarios) se pasan por referencia.

4. ¿Qué aprendiste? 

Lo que aprendemos aquí es que al ejecutar playingVector(position), no le estamos enviando a la función los números "6" y "9" sueltos; le estamos enviando la dirección de memoria donde vive el objeto position.

La variable v dentro de la función "apunta" al mismo lugar que la variable position fuera de ella. Por eso, cualquier cambio que hagas a las propiedades de v (como v.x = 20) afectará directamente al objeto original.

Esto es crucial cuando trabajas con sistemas de partículas o física. Si pasas un vector de posición a una función para calcular algo y lo modificas accidentalmente dentro, estarás alterando el movimiento de tu objeto de forma permanente.

---

**Actividad 5**

1. ¿Para qué sirve el método mag()? ¿Cuál es la diferencia con magSq() y cuál es más eficiente?

- mag(): Calcula la magnitud (longitud) del vector usando el teorema de Pitágoras.

- magSq(): Calcula la magnitud al cuadrado, es decir, omite el paso de la raíz cuadrada.

magSq() es mucho más eficiente. La operación de raíz cuadrada  es costosa para el procesador. Si solo necesitas comparar qué vector es más largo que otro, es mejor comparar sus magnitudes al cuadrado.

2. ¿Para qué sirve el método normalize()?

Sirve para convertir cualquier vector en un vector unitario (1) sin cambiar su dirección. Es fundamental cuando solo te interesa la dirección de una fuerza o movimiento, para luego escalarlo multiplicándolo por un valor específico.

3. Respuesta al periodista sobre dot() (Producto Punto):

El producto punto sirve para medir qué tan alineados están dos vectores: nos devuelve un número que nos dice si apuntan en la misma dirección, en direcciones opuestas o si son perpendiculares.

4. Diferencia entre la versión estática y de instancia de dot():

- De instancia (v1.dot(v2)): Se llama sobre un objeto vector ya existente. Es más común cuando estás operando directamente con una variable.

- Estática (p5.Vector.dot(v1, v2)): Se llama desde la "clase" global. Su principal ventaja es la legibilidad en fórmulas matemáticas complejas y que no requiere que operes sobre un objeto específico, sino que pasas ambos como argumentos. (Nota: En el caso de dot, ambas devuelven un número, no modifican el vector).

5. Intuición geométrica del Producto Cruz (cross()):

- Orientación: El resultado es un nuevo vector que es totalmente perpendicular al plano formado por los dos vectores originales (imagina un tornillo saliendo de una superficie).

- Magnitud: El tamaño del vector resultante es igual al área del paralelogramo que formarían los dos vectores originales. Si los vectores son paralelos, el producto cruz es cero.

6. ¿Para qué sirve el método dist()?

Sirve para calcular la distancia euclidiana entre dos puntos (tratando los vectores como posiciones). Es un atajo para restar un vector de otro y luego calcular la magnitud del resultado: p5.Vector.sub(v1, v2).mag().

7. ¿Para qué sirven los métodos normalize() y limit()?

Son las herramientas de control del movimiento:
- normalize(): Se usa para obtener la "dirección pura".

- limit(): Se usa para ponerle un techo o velocidad máxima a un vector. Si un vector de aceleración o velocidad crece demasiado, limit(max) asegura que su magnitud nunca supere ese valor, evitando que tus objetos "salgan volando" infinitamente por la pantalla.

---

**Actividad 6**

```java
let t = 0;
let step = 0.01;

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(220);

    // 1. Origen
    let v0 = createVector(20, 20);
    
    // 2. Definimos los vectores base
    let v1 = createVector(width - 50, 0);   // Rojo
    let v2 = createVector(0, height - 50);  // Azul
    
    // 3. Calculamos la interpolación de movimiento
    let vLerp = p5.Vector.lerp(v1, v2, t);

    // --- LÓGICA DE COLOR ---
    let colorRojo = color(255, 0, 0);   // Definimos color rojo
    let colorAzul = color(0, 0, 255);   // Definimos color azul
    // lerpColor mezcla los dos colores basándose en t (0.0 a 1.0)
    let colorDinamico = lerpColor(colorRojo, colorAzul, t);

    // 4. Dibujar vectores
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    
    // Usamos el color dinámico para la flecha que se mueve
    drawArrow(v0, vLerp, colorDinamico);

    // Vector verde
    let v01 = createVector(365, 25); 
    let v02 = createVector(-345, 350); 
    drawArrow(v01, v02, 'green');

    // Animación de ida y vuelta
    t += step;
    if (t > 1 || t < 0) step *= -1;
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 10;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
[[https://editor.p5js.org/](https://editor.p5js.org/zukoiheree/full/qU5oPJijX)](https://editor.p5js.org/zukoiheree/sketches/XTahd1AN6)

---

**Actividad 7**

El marco de trabajo "Motion 101" es la base fundamental que utiliza Daniel Shiffman en The Nature of Code para simular cualquier tipo de objeto en movimiento. Es una simplificación de las leyes de Newton aplicada al código.

Aquí tienes el análisis para tu bitácora:

1. ¿Cuál es el concepto del marco "Motion 101" y cómo se interpreta geométricamente?

Es un algoritmo de pasos constantes que se ejecutan en cada ciclo del dibujo (draw()) para actualizar el estado de un objeto. Se basa en una jerarquía de acumulación:

- La Aceleración cambia la Velocidad.
- La Velocidad cambia la Posición.

En el ejemplo 1.7, se centra solo en la segunda parte: Posición + Velocidad.

Posición: Es un vector que apunta desde el origen (0,0) hasta donde está el objeto. Es un "punto" en el espacio.

Velocidad: Es un vector que describe el desplazamiento por unidad de tiempo.

La suma: Geométricamente, sumar la velocidad a la posición es como colocar el inicio del vector velocidad en la punta del vector posición. El nuevo punto resultante es la ubicación del objeto en el siguiente cuadro de animación (frame).

Si la velocidad es constante, el objeto describe una línea recta.

2. ¿Cómo se aplica "Motion 101" en el ejemplo 1.7?

En el ejemplo 1.7, el marco de trabajo se aplica siguiendo estos cuatro pasos dentro de una clase o estructura de objeto:

Inicialización (Setup):
Se crean dos vectores: position (ubicación inicial aleatoria) y velocity (una dirección y magnitud constante, por ejemplo (2.5, 5)).
```java
code
JavaScript
download
content_copy
expand_less
position = createVector(100, 100);
velocity = createVector(2.5, 5);
```

Actualización (Update):
En cada frame, se aplica la regla de oro de Motion 101:
```java
code
JavaScript
download
content_copy
expand_less
position.add(velocity); // "Mueve" el objeto
```

Comprobación de bordes (Check Edges):
Se añade una lógica para que el objeto no desaparezca de la pantalla. Si la componente x o y de la posición supera los límites del lienzo, se invierte la dirección de la velocidad multiplicándola por -1.

Estás reflejando el vector velocidad respecto al eje de la pared.

Visualización (Display):
Se dibuja la forma (un círculo) usando las coordenadas del vector posición:
```java
code
JavaScript
download
content_copy
expand_less
ellipse(position.x, position.y, 48, 48);
```
---

**Actividad 8**

1. Aceleración Constante

- El código: acceleration = createVector(0.01, 0);

El objeto comienza a moverse lentamente y va ganando velocidad de forma lineal.
Se siente cinético y predecible. Es el comportamiento de un coche que pisa el acelerador a fondo o un objeto cayendo por gravedad.

La velocidad nunca deja de aumentar. Sin un límite (limit()), el objeto eventualmente se vuelve una ráfaga borrosa y desaparece. Es la base de cualquier simulación de física clásica.

2. Aceleración Aleatoria

- El código: acceleration = p5.Vector.random2D();

El objeto se mueve de forma errática, pero no es igual a una caminata aleatoria.
Se siente orgánico y "nervioso", como un insecto o una mosca.

3. Aceleración hacia el Mouse (Atracción)

Calcular dirección: dir = p5.Vector.sub(mouse, position);

- Normalizar: dir.normalize();

- Escalar: dir.mult(0.5);

- acceleration = dir;

- El efecto "Overshoot": Lo más interesante es que, si la aceleración es fuerte, el objeto suele pasar de largo (sobrepasa el mouse) porque lleva demasiada velocidad acumulada. Entonces debe dar la vuelta y regresar, creando un efecto de órbita o resorte alrededor del cursor. Nunca se detiene exactamente sobre el mouse, sino que "vibra" o gira a su alrededor.

## Bitácora de aplicación 

**Actividad 9**

1. Concepto de la obra:
La obra presenta un sistema de cientos de "partículas de luz" que habitan un vacío oscuro. El concepto central es la dualidad de la fuerza: la tensión entre la atracción (orden/unión) y la repulsión (caos/dispersión). La obra busca evocar una sensación orgánica, similar a la observación de microorganismos bajo un microscopio o estrellas en una nebulosa, donde el espectador es la entidad que altera el equilibrio del ecosistema con su presencia.

2. Regla de aceleración aplicada:
Para esta obra, apliqué una regla de Aceleración Proporcional Inversa al Cuadrado con Inversión de Polaridad.

La Regla: La aceleración no es constante ni aleatoria; se calcula restando la posición de la partícula de la posición del mouse (target - position).

Decisión de diseño: Elegí esta regla porque permite un comportamiento "elástico". Si el usuario mantiene el mouse quieto, las partículas orbitan; si lo mueve rápido, genera estelas.

Interacción: Al presionar el mouse, la regla se invierte: la aceleración se vuelve negativa, transformando el imán en un explosivo. Esta exploración artística busca mostrar cómo un cambio mínimo en un vector (un signo negativo) cambia completamente la narrativa visual de "atracción amorosa" a "rechazo violento".

```java
JavaScript
download
content_copy
expand_less
let particles = [];
let mode = 1; // 1: Atracción, -1: Repulsión

function setup() {
  createCanvas(windowWidth, windowHeight);
  // Crear 400 partículas en posiciones aleatorias
  for (let i = 0; i < 400; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  // Fondo con transparencia para crear estelas (trails)
  background(0, 25); 
  
  // Instrucciones en pantalla
  fill(255, 100);
  noStroke();
  text("MUEVE EL MOUSE para atraer | CLICK para repeler", 20, 30);

  let mousePos = createVector(mouseX, mouseY);

  for (let p of particles) {
    // 1. CALCULAR ACELERACIÓN (La regla de manipulación)
    let force = p5.Vector.sub(mousePos, p.pos);
    let distance = force.mag();
    distance = constrain(distance, 5, 100); // Evitar fuerzas infinitas
    
    force.normalize();
    
    // Si presionas el mouse, mode se vuelve -1 (Repulsión)
    let strength = (mouseIsPressed) ? -2 : 1;
    force.mult(strength * 0.5); 

    // 2. APLICAR MOTION 101
    p.applyForce(force);
    p.update();
    p.checkEdges();
    p.show();
  }
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxSpeed = 5;
    this.color = color(random(100, 255), random(100, 255), 255);
  }

  applyForce(f) {
    this.acc.add(f); // La aceleración acumula las fuerzas
  }

  update() {
    // REGLA MOTION 101:
    // Aceleración cambia Velocidad -> Velocidad cambia Posición
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed); // Limitar para que no escape al infinito
    this.pos.add(this.vel);
    
    // Limpiar aceleración en cada frame
    this.acc.mult(0);
  }

  show() {
    stroke(this.color);
    // El grosor depende de la velocidad (visualización de la energía)
    strokeWeight(this.vel.mag() * 1.5);
    point(this.pos.x, this.pos.y);
  }

  checkEdges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }
}
```
<img width="921" height="812" alt="image" src="https://github.com/user-attachments/assets/6a86335b-2b48-42a0-a487-e2d2cc9a7875" />


https://editor.p5js.org/zukoiheree/sketches/kNIwYG053

Instrucciones de uso:

Mover el mouse: Las partículas se sienten atraídas hacia el cursor como polillas a una luz, pero debido a su inercia (Motion 101), no se quedan quietas, sino que orbitan y crean patrones fluidos.

Hacer Click: La fuerza se invierte instantáneamente, dispersando la nube de partículas en una explosión geométrica.

---

## Bitácora de reflexión

**Actividad 10**

---

