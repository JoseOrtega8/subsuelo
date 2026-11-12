# 🕯️ SUBSUELO

> Plataformero de precisión minimalista hecho en JavaScript puro.
> Guía a **Chispa**, una pequeña brasa perdida en una mina abandonada bajo el desierto de Sonora, hasta la superficie.

![Estado](https://img.shields.io/badge/estado-propuesta-orange)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow)
![Licencia](https://img.shields.io/badge/licencia-MIT-blue)

🎮 **Jugar en línea:** https://joseortega8.github.io/subsuelo/ *(la versión jugable se publica cuando termine el desarrollo)*

---

## 📌 Estado del proyecto

En este repositorio por ahora **solo está la propuesta**. El código se desarrolla y se libera aquí mismo en las siguientes entregas.

---

## 1. Propuesta

### Concepto
**Subsuelo** es un juego web indie de plataformas en 2D donde lo importante es la **habilidad y el timing**: cada salto cuenta. Se inspira en el movimiento de juegos como *Hollow Knight* y *Celeste* (salto preciso, dash, rebote sobre peligros), pero con personaje, historia y estilo visual propios, y con un alcance pequeño que se pueda terminar.

### Historia (mínima)
Una explosión en una vieja mina de Sonora dejó a **Chispa**, una brasa con vida, atrapada en lo más profundo. Su luz se apaga si toca los picos o cae al vacío. Tiene que subir sala por sala hasta volver a ver el sol del desierto.

### Objetivo del jugador
Cruzar todas las salas de la mina hasta la salida con **el menor número de muertes y en el menor tiempo posible**. Morir no castiga mucho: Chispa reaparece al instante en la última linterna (checkpoint), así que el jugador puede intentarlo una y otra vez.

---

## 2. Mecánicas principales

| Mecánica | Descripción | Se desbloquea |
|---|---|---|
| **Correr y saltar** | Movimiento horizontal con aceleración; salto de altura variable (si sueltas la tecla antes, saltas menos). | Sala 1 |
| **Deslizar y saltar en pared** | Chispa se pega a las paredes, resbala despacio y puede impulsarse hacia el lado contrario. | Sala 2 |
| **Dash** | Impulso rápido en la dirección que mira. **1 uso en el aire**, se recarga al tocar el suelo. | Sala 3 |
| **Pogo (rebote)** | Atacar hacia abajo sobre picos o enemigos hace rebotar a Chispa y le recarga el dash. | Sala 4 |
| **Linternas** | Checkpoints que guardan el punto de reaparición. | Todas |

### Peligros
- **Picos:** muerte instantánea (salvo que se haga pogo encima).
- **Estalactitas:** caen cuando el jugador pasa debajo.
- **Plataformas que se desmoronan:** desaparecen 0.5 s después de pisarlas.
- **Polillas de mina:** enemigo simple que patrulla de lado a lado.

---

## 3. "Game feel": por qué se va a sentir bien

Que un plataformero se sienta justo depende de detalles técnicos que el jugador no ve:

- **Coyote time (~100 ms):** todavía se puede saltar un instante después de salirse de una orilla.
- **Jump buffer (~100 ms):** si se presiona saltar poco antes de tocar el suelo, el salto se registra igual.
- **Gravedad más fuerte al caer:** la caída es más rápida que la subida, y el salto se siente firme, no "flotado".
- **Velocidad máxima de caída:** se puede reaccionar siempre.
- **Pequeños efectos visuales:** partículas al aterrizar o al hacer dash, y una sacudida de pantalla corta al morir.

---

## 4. Controles

| Acción | Teclado |
|---|---|
| Moverse | `←` `→` o `A` `D` |
| Saltar | `Espacio` o `Z` |
| Dash | `Shift` o `X` |
| Atacar / Pogo | `C` (+ `↓` para atacar hacia abajo) |
| Pausa | `Esc` |
| Reiniciar sala | `R` |

---

## 5. Contenido y niveles

- **6 salas** del tamaño de una pantalla, conectadas en vertical (de abajo hacia arriba).
- Cada sala **enseña una mecánica nueva** y la siguiente la combina con las anteriores (tutorial sin texto).
- La última sala es un **reto final** que mezcla todas las habilidades.
- **Pantalla final** con tiempo total y número de muertes.

---

## 6. Estilo visual minimalista

- Resolución interna de **320 × 180 px**, escalada a pantalla completa con píxeles nítidos.
- **Paleta de 4 colores:** negro de mina, gris roca, ocre del desierto y naranja brasa (Chispa y las linternas).
- Todo dibujado con **figuras geométricas en Canvas**, sin imágenes externas.
- La luz de Chispa ilumina un círculo a su alrededor y el resto de la mina queda en penumbra.

---

## 7. Audio

Efectos sencillos **generados por código con la Web Audio API** (osciladores), sin archivos de sonido:

| Evento | Sonido |
|---|---|
| Salto | "blip" corto que sube de tono |
| Dash | ráfaga de ruido corta |
| Pogo | "ping" metálico |
| Muerte | tono grave que baja |
| Linterna | acorde corto y brillante |
| Ambiente | zumbido grave y constante de la mina (se puede silenciar con `M`) |

---

## 8. Tecnologías y arquitectura

- **HTML5 Canvas** + **JavaScript (ES6, módulos)**, sin librerías ni frameworks.
- **Bucle de juego con paso fijo (60 actualizaciones/s)**: la física se comporta igual en cualquier computadora.
- **Mapas como arreglos de tiles** (cada número representa un bloque: suelo, pico, plataforma, etc.).
- **Colisiones AABB** (rectángulos alineados a los ejes), resueltas eje por eje.
- **Máquina de estados** para el jugador: `suelo`, `aire`, `pared`, `dash`, `muerto`.
- Se publica con **GitHub Pages**, se juega directo en el navegador.

### Estructura planeada

```
subsuelo/
├── index.html
├── style.css
├── README.md
└── src/
    ├── main.js        # Bucle principal y arranque
    ├── input.js       # Teclado y buffer de entradas
    ├── player.js      # Física y estados de Chispa
    ├── level.js       # Carga de salas y tiles
    ├── levels.js      # Datos de las 6 salas
    ├── collision.js   # Colisiones AABB
    ├── entities.js    # Enemigos, estalactitas, plataformas
    ├── audio.js       # Efectos con Web Audio API
    ├── render.js      # Dibujo, luz y partículas
    └── ui.js          # Menú, pausa, pantalla final
```

---

## 9. Alcance

**Incluye:**
- 6 salas jugables, 4 habilidades de movimiento, 4 tipos de peligro
- Checkpoints, contador de muertes y cronómetro
- Efectos de sonido y ambiente
- Versión jugable en línea

**No incluye** (para mantener el proyecto terminable):
- Mapa abierto tipo metroidvania o exploración no lineal
- Sistema de vida, combate complejo o jefes
- Guardado de partida entre sesiones
- Controles táctiles para celular

---

## 10. Plan de desarrollo

| Fase | Entregable |
|---|---|
| 1. Propuesta | Repositorio + README (esta entrega) |
| 2. Base | Canvas, bucle de juego, entrada y movimiento básico con colisiones |
| 3. Movimiento | Salto variable, coyote time, buffer, wall jump, dash y pogo |
| 4. Contenido | Las 6 salas, peligros, enemigos y checkpoints |
| 5. Pulido | Audio, luz, partículas, menús y pantalla final |
| 6. Publicación | GitHub Pages + video explicativo del código |

---

## 11. Video explicativo

🎥 *Pendiente: se agrega aquí cuando el código esté terminado.*

---

## 12. Licencia

Código libre bajo licencia **MIT**: cualquiera puede usarlo, estudiarlo y modificarlo.

---

## 👤 Autor

**José Carlos Ortega**
Ingeniería en Desarrollo y Gestión de Software Multiplataforma, Universidad Tecnológica de Hermosillo
GitHub: [@JoseOrtega8](https://github.com/JoseOrtega8)
