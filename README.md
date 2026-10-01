# Guiñote online

Juego de guiñote con baraja española para 2 jugadores, en un único `index.html` (HTML, CSS y JavaScript). Usa Firebase Realtime Database para sincronizar la partida en tiempo real.

## Estructura del proyecto

```
baraja/
├── index.html
├── config.dat
├── README.md
└── cartas/
    ├── oros_01.webp … oros_12.webp
    ├── copas_*.webp, espadas_*.webp, bastos_*.webp
    └── reverso.webp
```

Solo se usan las cartas 1–7 y 10–12 de cada palo (baraja de 40). Las imágenes `_08` y `_09` y `blanco.webp` no se emplean.

## Puesta en marcha

### 1. Servir la carpeta

El `fetch` de la configuración no funciona con `file://`, así que usa un servidor web:

```bash
cd ~/projects/baraja
python3 -m http.server 8000
```

Abre `http://localhost:8000`. Para jugar con otra persona, publica la carpeta en cualquier hosting estático (Firebase Hosting, GitHub Pages, Netlify…) o usa un túnel.

### 2. Jugar

1. Un jugador pulsa **Crear sala** y comparte el código de 4 letras.
2. El otro lo escribe y pulsa **Unirse a la sala**.
3. La partida se reparte sola cuando entra el segundo jugador.

Si recargas la página, vuelves automáticamente a tu sala.

## Reglas implementadas

**Baraja y valores**

| Carta         | Fuerza     | Puntos |
| ------------- | ---------- | ------ |
| As            | 1.ª        | 11     |
| Tres          | 2.ª        | 10     |
| Rey           | 3.ª        | 4      |
| Caballo       | 4.ª        | 3      |
| Sota          | 5.ª        | 2      |
| 7, 6, 5, 4, 2 | 6.ª a 10.ª | 0      |

**Desarrollo**

- Cada jugador recibe 6 cartas. La carta que queda visible bajo el mazo marca el palo de triunfo.
- Gana la baza la carta más alta del palo de salida, o el triunfo más alto si se falla con triunfo. Quien la gana roba primero y sale en la siguiente.
- **Mientras hay mazo**, se puede jugar cualquier carta.
- **En el arrastre** (mazo agotado) hay que asistir al palo, superar la carta del rival si se puede y, si no se tiene el palo, fallar con triunfo si se tiene. Las cartas no permitidas se muestran atenuadas.
- **Cantes:** quien tiene una baza ganada y sale puede cantar rey y caballo del mismo palo: 20 puntos, o 40 si es de triunfo. Después debe salir con una de esas dos cartas. Cada palo se canta una sola vez.
- **Cambiar el 7:** quien tiene una baza ganada y sale puede cambiar el 7 de triunfo por la carta vista, mientras queden al menos 2 cartas en el mazo.
- **Diez de últimas:** quien gane la última baza suma 10 puntos.
- Solo ves tus propios puntos durante la partida. Al terminar se muestran los dos y gana quien tenga más.
