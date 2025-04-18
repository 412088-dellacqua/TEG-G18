# TEG
# Grupo 18

# 🧱 Entidades Principales del TEG

---

## 🎮 Jugador

**Responsabilidad:** Representa a un jugador en la partida (puede ser humano o bot (hereda)).

**Atributos:**
- `id: Long`
- `nombre: String`
- `tipo: enum TipoJugador { HUMANO, BOT_NOVATO, BOT_BALANCEADO, BOT_EXPERTO }`
- `ejercitosDisponibles: int`
- `posicionRonda: int`<!-- Representa la posición del jugador en la ronda, sorteada por la partida, y sirve para recargar partida -->
- `color: enum Colores { CELESTE, AMARILLO, ROJO, ROSA, VERDE, NEGRO }`
- `objetivoSecreto: Objetivo`
- `objetivoComun: Objetivo`
- `paisesOcupados: List<Pais>`
- `eliminado: boolean`

**Métodos:**
- `colocarEjercitos(Map<Pais, Integer> distribucion)`
- `atacar(Pais origen, Pais destino, int cantidadEjercitos)`
- `defender(int cantidadEjercitos)`
- `reagrupar(Pais origen, Pais destino, int cantidadEjercitos)`
- `solicitarCarta()`
- `canjearCartas()`
- `verificarObjetivoCumplido()`
- `obtenerCantidadTotalEjercitos()`

---

## 🌍 País

**Responsabilidad:** Representa un territorio del mapa.

**Atributos:**
- `id: Long`
- `nombre: String`
- `continente: Continente`
- `ocupante: Jugador`
- `ejercitos: int`
- `paisesLimitrofes: List<Pais>`

**Métodos:**
- `esLimitrofeCon(Pais otro)`
- `agregarEjercitos(int cantidad)`
- `quitarEjercitos(int cantidad)`

---

## 🗺️ Continente

**Responsabilidad:** Agrupa un conjunto de países, ofrece bonificación de ejércitos.

**Atributos:**
- `id: Long`
- `nombre: String`
- `paises: List<Pais>`

**Métodos:**
- `estaControladoPor(Jugador jugador)`
- `cantidadPaisesControladosPor(Jugador jugador)`

---

## 🎯 Objetivo

**Responsabilidad:** Define la condición de victoria secreta o común.

**Atributos:**
- `id: Long`
- `descripcion: String`
- `tipo: enum TipoObjetivo { COMUN, SECRETO }`

**Métodos:**
- 

---

## 🕹️ Partida

**Responsabilidad:** Gestiona el ciclo de vida del juego.

**Atributos:**
- `id: Long`
- `jugadores: List<Jugador>`
- `mapa: Mapa` <!-- Se puede insertar una vez se ejecute metodo cargarMapa() -->
- `tipoPartida: enum TiposDePartida { FAIR_PLAY, VALE_TODO }`
- `estado: enum EstadoPartida { INICIAL, EN_CURSO, FINALIZADA }`
- `turnoActual: int`
- `numeroRonda: int` <!-- Contador de rondas -->
- `historialEventos: List<Evento>`
- `tiempoPorTurno: int` (segundos)

**Métodos:**
- `iniciarPartida()`
- `crearJugador()` <!-- Se le asigna color -->
- `sortearTurno()` <!-- Se le asigna turnos al jugador -->
- `repartirCartas()` <!-- Se mezclan las cartas adentro de este metodo, también reparte objetivos -->
- `cargarMapa()` <!-- Se carga tablero -->
- `avanzarTurno()` <!-- Validar primero si se cumplió un objetivo al avanzar turno, si pasa finalizar la misma -->
- `verificarGanador()`
- `guardarEstado()` <!-- Se guarda el momento actual de la partida -->
- `cargarEstado()` <!-- Se carga el momento actual de la partida -->
- `finalizarPartida()` <!-- Fin -->
---

## 🧭 Mapa

**Responsabilidad:** Representa el tablero con todos los países y continentes.

**Atributos:**
- `paises: List<Pais>`
- `continentes: List<Continente>`

**Métodos:**
- `repartirPaises(List<Jugador> jugadores)` <!-- Reparte paises a los X jugadores -->
- `validarEventoJugador(Pais origen, Pais destino, Jugador_en_curso, Evento)` <!-- Comunica las acciones del jugador(ataque, reagrupar), asigna fichas a los paises --> <!-- Validar si es limitrofe -->

---

## 🃏 TarjetaPais

**Responsabilidad:** Tarjeta que se usa para canjear por ejércitos.

**Atributos:**
- `id: Long`
- `pais: Pais`
- `simbolo: enum Simbolo { GALEÓN, GLOBO, CAÑON }`
- `jugadorActual: Jugador` (si está en su poder)

**Métodos:**
-

---

## 📝 Evento

**Responsabilidad:** Representa un movimiento o acción dentro de la partida.

**Atributos:**
- `id: Long`
- `descripcion: String`
- `fechaHora: LocalDateTime`
- `tipo: enum TipoEvento { ATAQUE, REFUERZO, DEFENSA, REAGRUPACION, OBJETIVO_CUMPLIDO, PARTIDA_FINALIZADA, ORGANIZAR_MAPA }`

**Métodos:**  
- `insertarEvento()` <!-- Insertar eventos de la partida -->

*No requiere muchos métodos, es solo para logging/historial.*
*Revisar metodo para guardar en un atributo historial un objeto*
---

## 🎲 Dados

**Responsabilidad:** Simula los enfrentamientos entre países durante los ataques.

**Atributos:**
- `valoresDado: List<Integer>` — resultados de los dados del atacante (máx. 3)

**Métodos:**
- `lanzarDados(int cantidad)`

---

## Comunicación

**Responsabilidad:**  Simula el chat entre bots y jugadores

**Atributos:**
- `id: Long`
- `mensaje: String`

**Métodos:**
- `chatGeneral()` <!-- Insertar dialogo en público @everyone -->
- `chatPrivado()` <!-- Insertar dialogo en privado @paolini_pachequini -->

---

## 🌍 Reglas

**Responsabilidad:** Representa reglamento general

**Atributos:**
- `id: Long`
- `nombre: String`


**Métodos:**
- `obtenerReglas()`

