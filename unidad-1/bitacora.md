# Unidad 1

## Bitácora de proceso de aprendizaje
**Actividad 1**

En el arte generativo, la aleatoriedad actúa como un colaborador autónomo que introduce variaciones infinitas y sorpresas dentro de un sistema de reglas, permitiendo que la obra final trascienda el control absoluto del artista y adquiera una vida propia e impredecible.

---
**Actividad 2**

Modificacion:
```java
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;
// let walker2;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  // walker2 = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
  
  // walker2.step();
  // walker2.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    // circle(this.x, this.y,20);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
Lo que se espera es caminata aleatoria tradicional con dos **círculos** independientes parten del centro y se mueven de forma errática
<img width="645" height="241" alt="image" src="https://github.com/user-attachments/assets/681f4e5a-3271-40c9-b9f6-55e58e17e470" />

---
**Actividad 3**

- Uniforme: Es "democrática". No hay preferencias; cualquier número dentro del rango tiene la misma chance.[2]
- No Uniforme: Es "selectiva". Hay zonas donde los números aparecen con mucha frecuencia y otras donde son muy raros.

<img width="650" height="247" alt="image" src="https://github.com/user-attachments/assets/0f833cec-1dc3-4cd4-9d35-d3aaa7a066bf" />


En este código específico, los círculos se mueven a la derecha debido a la acumulación de probabilidades y a la ausencia de un movimiento compensatorio.

Si analizamos el método step(), vemos que hay 4 posibles resultados para la variable choice (0, 1, 2 y 3), cada uno con un 25% de probabilidad. Veamos qué hace cada uno:

- if (choice == 0): this.x++; **(Mueve a la Derecha)**
- else if (choice == 1): this.x++; **(Mueve a la Derecha)**
- else if (choice == 2): this.y++; **(Mueve hacia Abajo)**
- else: this.y--; **(Mueve hacia Arriba)**

El código asigna tanto el caso 0 como el caso 1 para incrementar x. Esto significa que el Walker tiene un 50% de probabilidad (25% + 25%) de moverse a la derecha en cada paso.

---
**Actividad 4**

```java
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  // 1. La "distancia" al centro sigue una distribución normal
  // Media 0 (para que tienda a estar en el centro) 
  // Desviación estándar 60 (qué tan dispersa es la nube)
  let dist = randomGaussian(0, 60);
  
  // 2. Elegimos un ángulo aleatorio uniforme (0 a 360 grados)
  let angulo = random(TWO_PI);

  // 3. Convertimos coordenadas polares (distancia y ángulo) a cartesianas (x, y)
  // Sumamos width/2 y height/2 para que el centro sea la mitad de la pantalla
  let x = width / 2 + cos(angulo) * dist;
  let y = height / 2 + sin(angulo) * dist;

  noStroke();
  
  // 4. Cambiamos el color según la distancia para resaltar la distribución
  // Si está cerca del centro (distancia pequeña), es más oscuro
  fill(0, 100); 
  
  circle(x, y, 4);
}
```
<img width="635" height="238" alt="image" src="https://github.com/user-attachments/assets/9c8d1a7a-dae5-4d67-8941-81c1d12f6f66" />

---
**Actividad 5**

Voy a modificar el código del Walker (caminante aleatorio) utilizando la técnica de Vuelo de Lévy.

En lugar de que el caminante se mueva siempre un píxel a la vez, utilizaremos una distribución personalizada para decidir el tamaño del paso. La mayoría de los pasos serán muy cortos, pero ocasionalmente el caminante dará un salto gigante.
```java
// The Nature of Code - Daniel Shiffman
// Implementación de Lévy Flight usando Aceptación-Rechazo

let walker;

function setup() {
  createCanvas(640, 480);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.px = this.x; // Guardamos la posición anterior para dibujar líneas
    this.py = this.y;
  }

  show() {
    stroke(0, 100);
    strokeWeight(2);
    line(this.px, this.py, this.x, this.y); // Dibujamos una línea para ver el salto
  }

  step() {
    this.px = this.x;
    this.py = this.y;

    // 1. Obtenemos un tamaño de paso basado en la técnica de Lévy
    let stepSize = this.montecarlo() * 50; // Multiplicamos por 50 para exagerar los saltos

    // 2. Elegimos una dirección aleatoria (360 grados)
    let angle = random(TWO_PI);
    let stepX = cos(angle) * stepSize;
    let stepY = sin(angle) * stepSize;

    this.x += stepX;
    this.y += stepY;

    // Mantener dentro de la pantalla
    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }

  // Técnica de Aceptación-Rechazo (Monte Carlo) para Lévy Flight
  montecarlo() {
    while (true) {
      let r1 = random(1); // Candidato a tamaño de paso
      let probability = pow(1 - r1, 8); // Probabilidad: Muy alta para pasos cortos, bajísima para largos
      
      let r2 = random(1); // El "examen"
      
      if (r2 < probability) {
        return r1;
      }
    }
  }
}
```
<img width="547" height="478" alt="image" src="https://github.com/user-attachments/assets/fd1e4d94-e3bc-4f56-8589-bc8f832f31aa" />

---
**Actividad 6**

Este código crea la silueta de un paisaje montañoso que fluye suavemente a través de la pantalla. En lugar de usar valores al azar que harían saltar la línea de forma caótica, utiliza la función Perlin Noise para calcular alturas que están relacionadas entre sí, lo que genera subidas y bajadas naturales y continuas. Al final de cada dibujo, el programa avanza un poco en una variable de tiempo, logrando que las montañas parezcan desplazarse u ondular de forma orgánica, como si estuviéramos observando un horizonte desde un vehículo en movimiento.

```java
let tiempo = 0; // Esta es nuestra "llave" para movernos en el ruido

function setup() {
  createCanvas(360, 240);
}

function draw() {
  background(200, 220, 255); // Un color cielo clarito
  
  stroke(0);        // Línea negra
  fill(50, 150, 50); // Color verde pasto
  strokeWeight(2);

  // Empezamos a dibujar la forma de la montaña
  beginShape();
  
  // Ponemos un punto en la esquina inferior izquierda
  vertex(0, height);

  // Recorremos todo el ancho de la pantalla
  for (let x = 0; x < width; x++) {
    
    // AQUÍ ESTÁ EL TRUCO:
    // noise() necesita un número que cambie poquito para ser suave.
    // Usamos 'x' para la forma y 'tiempo' para que se mueva.
    let altura = noise(x * 0.01, tiempo) * height;
    
    // Dibujamos el punto en esa posición
    vertex(x, altura);
  }

  // Cerramos la forma en la esquina inferior derecha para que se pinte el relleno
  vertex(width, height);
  endShape(CLOSE);

  // Avanzamos un poquito el tiempo para que las montañas "se muevan"
  tiempo += 0.01; 
}
```
<img width="358" height="239" alt="image" src="https://github.com/user-attachments/assets/2fcb0f19-eb37-44cd-a6c1-08dac9f2da54" />

Video prueba: https://drive.google.com/file/d/1nviBYmM5ie825uB7qmiOJaP9unr9QA9G/view?usp=sharing

---

## Bitácora de aplicación 
**Actividad 7**

- Perlin Noise (Movimiento y Forma): El cuerpo del organismo y su suave balanceo por la pantalla están dictados por ruido de Perlin, lo que le da una cualidad líquida y orgánica.
- Distribución Normal / Gausiana (Bruma de Partículas): Alrededor del organismo se genera una "bruma" de luz. La posición de estas partículas sigue una campana de Gauss para que estén muy concentradas cerca del núcleo y se desvanezcan suavemente hacia afuera.
- Lévy Flight (Interacción de Rayos): Cuando haces clic con el mouse, el organismo libera "descargas eléctricas". Estas descargas utilizan el Vuelo de Lévy: muchos segmentos cortos que de repente dan un salto largo, simulando el comportamiento de un rayo o un impulso nervioso.

```java
let t = 0; // Tiempo para Perlin Noise
let descargas = []; // Almacena los rayos de Lévy

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(10);
}

function draw() {
  // Fondo con rastro para efecto de luz
  background(10, 30); 

  // --- 1. MOVIMIENTO CON PERLIN NOISE ---
  // El núcleo se mueve suavemente por la pantalla
  let posX = noise(t) * width;
  let posY = noise(t + 1000) * height;
  
  // Si el mouse está cerca, el núcleo intenta seguirlo suavemente
  let targetX = mouseX;
  let targetY = mouseY;
  posX = lerp(posX, targetX, 0.05);
  posY = lerp(posY, targetY, 0.05);

  // --- 2. BRUMA CON DISTRIBUCIÓN GAUSIANA ---
  // Dibujamos partículas de luz concentradas en el centro
  noStroke();
  for (let i = 0; i < 20; i++) {
    // La posición X e Y se desvían de la media (posX, posY) de forma gausiana
    let x = randomGaussian(posX, 30); 
    let y = randomGaussian(posY, 30);
    
    let tamaño = random(1, 4);
    fill(0, 200, 255, 150);
    circle(x, y, tamaño);
  }

  // --- 3. DIBUJAR EL NÚCLEO (Perlin en el radio) ---
  fill(255, 255, 255, 200);
  let radioNucleo = noise(t * 2) * 40 + 20;
  circle(posX, posY, radioNucleo);

  // --- 4. VUELO DE LÉVY (Descargas eléctricas) ---
  // Si hay descargas activas, las dibujamos
  for (let d of descargas) {
    d.update();
    d.show();
  }

  t += 0.01;
}

// INTERACCIÓN: Al hacer clic, se crea un rayo con Vuelo de Lévy
function mousePressed() {
  descargas.push(new RayoLévy(mouseX, mouseY));
  // Limpiar array para no saturar
  if (descargas.length > 5) descargas.shift();
}

class RayoLévy {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.historia = [{x: x, y: y}];
  }

  update() {
    // Algoritmo de Lévy Flight para el siguiente paso
    let r = random(1);
    let stepSize;
    
    // 5% de probabilidad de un salto gigante, 95% de pasos cortos
    if (r < 0.05) {
      stepSize = random(50, 150); // Salto largo
    } else {
      stepSize = random(2, 10);   // Paso corto
    }

    let angulo = random(TWO_PI);
    this.x += cos(angulo) * stepSize;
    this.y += sin(angulo) * stepSize;
    
    this.historia.push({x: this.x, y: this.y});
    
    if (this.historia.length > 20) this.historia.shift();
  }

  show() {
    stroke(255, 255, 0, 200);
    strokeWeight(2);
    noFill();
    beginShape();
    for (let p of this.historia) {
      vertex(p.x, p.y);
    }
    endShape();
  }
}
```
<img width="852" height="814" alt="image" src="https://github.com/user-attachments/assets/acba0db9-8245-4dad-a874-4bdd7fa58418" />

Video Prueba: https://drive.google.com/file/d/1fDJuUB-uCDNnsQrParwDRGKf3bu7pqXP/view?usp=sharing


---


## Bitácora de reflexión
**Actividad 8**

1. Diferencia entre random() y noise(). ¿Cuándo usar cada uno?

Diferencia: La función random() genera valores independientes; el resultado actual no tiene ninguna relación con el anterior, lo que produce una apariencia caótica y "nerviosa". El Ruido Perlin (noise()), en cambio, genera una secuencia de valores relacionados entre sí; es un azar suave donde los números evolucionan de forma continua.

2. ¿Qué es una distribución de probabilidad? Diferencia visual entre Uniforme y Normal.

Es una regla o función que indica qué tan probable es que ocurra cada uno de los resultados posibles en un evento aleatorio. Es decir, nos dice qué números saldrán con más frecuencia.

En una caminata uniforme, el caminante tiene la misma probabilidad de dar pasos cortos o largos (o de ir en cualquier dirección), lo que hace que explore el espacio de forma equitativa y plana. En una caminata con distribución normal, el caminante tiende a quedarse mucho más cerca de su punto de inicio, ya que los valores cercanos al promedio (cero o pasos pequeños) son muchísimo más frecuentes que los pasos grandes en los extremos.

3. El papel de la aleatoriedad en el arte generativo y sus funciones.

- La aleatoriedad actúa como un colaborador autónomo. Permite que el artista pase de "dibujar" a "programar un sistema" que puede tomar sus propias decisiones.
- Permite que la obra genere versiones únicas cada vez que se ejecuta, evitando que sea una imagen estática y repetitiva.
- Ayuda a romper la perfección rígida de las computadoras, introduciendo "imperfecciones" y comportamientos que imitan la vida y la naturaleza.

4. Concepto de aleatoriedad usado en tu obra (Actividad 07).

Concepto: El Ruido Perlin.

Fue la elección adecuada para lograr el movimiento del organismo central. Como el objetivo era que pareciera una entidad viva y "consciente", el ruido Perlin permitió que se desplazara con una suavidad líquida. Si hubiera usado random(), el organismo se sacudiría de forma violenta y artificial; con Perlin, el movimiento tiene una inercia y una dirección que el ojo humano interpreta como algo natural.

5. ¿Qué es una “caminata” (walk) y qué caracteriza al “Lévy flight”?

Caminata aleatoria: Es una simulación donde un objeto se mueve paso a paso, y cada nueva posición se calcula sumando un valor azaroso a la posición anterior. Es básicamente una sucesión de pasos en el tiempo.

Lévy Flight: Se caracteriza por tener una distribución de "cola pesada". Esto significa que el caminante realiza muchos pasos pequeños y agrupados (explorando una zona a fondo) y, de forma repentina y ocasional, da un salto muy largo hacia una nueva zona. Visualmente, parece una mezcla de pequeños garabatos conectados por líneas largas.



