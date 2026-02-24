# Unidad 3

## Bitácora de proceso de aprendizaje

**Actividad 01**

---

**Actividad 02**

- La Aceleración es un "Estado Instantáneo": Aprendí que la aceleración no es como la velocidad. La velocidad tiene "memoria" (inercia), pero la aceleración solo existe mientras la fuerza esté empujando. Por eso, el comando acceleration.mult(0) al final del update() es vital: limpia el pasado para que en cada nuevo frame las fuerzas tengan que "ganarse" de nuevo el derecho a mover al objeto. Sin este reset, la física se rompe y el mundo se vuelve caótico.

- El Peligro de la "Referencia" (El Efecto Mariposa): Esta es la lección técnica más importante: en JavaScript, los vectores son objetos y se pasan por referencia. Si modifico una fuerza directamente dentro de un objeto (force.div(mass)), estoy alterando esa fuerza para todo el universo. Es una lección de humildad algorítmica: si quieres que un objeto sea afectado por su propia masa, debes crear una copia de la fuerza o usar métodos estáticos para no destruir la realidad de los demás objetos.

- La Masa como Escudo de la Inercia: Al integrar la masa (a=F/m), la simulación deja de ser plana. Ahora, un objeto con masa grande se vuelve "terco" ante las fuerzas (acelera lento), mientras que uno pequeño es "volátil". Esto permite que una misma fuerza (como el viento) genere comportamientos visuales distintos y orgánicos, simplemente cambiando un parámetro de escala.

---

**Actividad 03**

- Friccion

En esta obra, cientos de partículas son lanzadas con una explosión inicial de energía. El lienzo no es un vacío, sino que actúa como una superficie rugosa (como arena o papel).

<img width="942" height="749" alt="image" src="https://github.com/user-attachments/assets/70405b9f-8c2f-4a49-8ba2-7fbaaf9a6576" />
https://editor.p5js.org/zukoiheree/sketches/xesVnKKfX

- Resistencia al aire y fluidos

Representa la caída de cuerpos en un entorno dividido. La parte superior es el vacío (aire), donde la gravedad actúa libremente. La parte inferior es un fluido denso y oscuro.
La obra busca visualizar cómo la energía cinética se transforma al cambiar de medio. 

Aquí, el factor crítico es que la fuerza es proporcional al cuadrado de la velocidad. Esto crea una "velocidad terminal": el punto donde la gravedad y la resistencia se equilibran y el objeto deja de acelerar.
<img width="942" height="749" alt="image" src="https://github.com/user-attachments/assets/9085a0d0-7a82-4297-95a8-aa0f53d23802" />
https://editor.p5js.org/zukoiheree/sketches/KrhHxZWcM

- Atracción gravitacional

En lugar de ver planetas orbitando, imagina que estamos viendo filamentos de materia oscura. En esta obra, las partículas son casi invisibles, pero tienen "memoria".

Contiene:
1. Atracción Nómada: Los centros de gravedad no están quietos; se desplazan por el lienzo siguiendo un Ruido de Perlin, creando un campo gravitatorio que muta constantemente como si el espacio mismo se estuviera doblando.
2. Repulsión de Proximidad: He añadido una regla "anti-Ventrella": las partículas son atraídas por los grandes centros de masa, pero se repelen entre sí si están demasiado cerca. Esto evita que se amontonen y crea estructuras filamentosas similares a redes neuronales o tejidos cósmicos.
3. Estética de Carboncillo: Se eliminan los círculos. Solo vemos líneas finas que se cruzan. El resultado parece un dibujo a mano alzada realizado por la propia gravedad.
<img width="941" height="748" alt="image" src="https://github.com/user-attachments/assets/29bcfd41-ea90-4db1-a0ef-323ef2e6fc63" />
https://editor.p5js.org/zukoiheree/sketches/6LtAx8FD2

---

## Bitácora de aplicación 

**Actividad 04**
Imagina que dentro de una computadora antigua hay un proceso eterno: el sistema intenta organizar "fragmentos de memoria" en una columna central perfecta. Sin embargo, el sistema está degradado.

La historia trata sobre la resistencia de la información ante el caos.

- Los Fragmentos (Vectores): Son pequeñas líneas de código o datos que buscan el orden.
- La Columna (Atracción Gravitatoria): Existe una fuerza magnética en el centro de la pantalla que intenta alinear todos los fragmentos verticalmente.
- El Vacío (Ruido y Viento): Una fuerza constante de interferencia intenta empujar los datos hacia los márgenes, hacia el olvido.
- El Usuario (El Corruptor/Restaurador): Tu presencia (el mouse) es una anomalía. Al pasar cerca, generas una repulsión violenta que desordena el archivo, pero al hacer clic, generas un pulso de sincronización que reinicia la masa de los fragmentos, dándoles una nueva oportunidad de llegar al centro.

Cómo las fuerzas moldean las reglas:

- Aceleración de Alineación: No es solo atracción al centro, es una fuerza que actúa más fuerte en el eje X que en el Y, obligando a las partículas a formar una "columna" de datos.
- Masa Diferencial: Los fragmentos más largos son más "pesados" (mayor masa). Esto implica que la fuerza del mouse los afecta menos, pero también les cuesta más trabajo regresar al archivo central cuando la tormenta los empuja.
- Fricción de Datos: He añadido una fricción muy alta. Esto hace que el movimiento no sea fluido como el agua, sino tosco y mecánico, como si las partículas estuvieran moviéndose a través de un sistema operativo viejo y pesado.

<img width="941" height="748" alt="image" src="https://github.com/user-attachments/assets/79887903-e20c-4f08-9427-99643aa958f7" />
https://editor.p5js.org/zukoiheree/sketches/f2T_gD3w3

## Bitácora de reflexión

**Actividad 05**
