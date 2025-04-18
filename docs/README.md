# TEG
# Grupo 18

# 🧱 Entidades Principales del TEG

---

## 🎮 Jugador

**Responsabilidad:** Representa a un jugador en la partida (puede ser humano o bot).

**Atributos:**
- `id: Long`
- `nombre: String`
- `tipo: enum TipoJugador { HUMANO, BOT_NOVATO, BOT_BALANCEADO, BOT_EXPERTO }`
- `ejercitosDisponibles: int`
- `objetivoSecreto: Objetivo`
- `paisesOcupados: List<Pais>`
- `eliminado: boolean`

**Métodos:**
- `colocarEjercitos(Map<Pais, Integer> distribucion)`
- `atacar(Pais origen, Pais destino, int cantidadEjercitos)`
- `reagrupar(Pais origen, Pais destino, int cantidadEjercitos)`
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
- `puedeSerAtacadoPor(Jugador atacante)`
- `agregarEjercitos(int cantidad)`
- `quitarEjercitos(int cantidad)`

---

## 🗺️ Continente

**Responsabilidad:** Agrupa un conjunto de países, ofrece bonificación de ejércitos.

**Atributos:**
- `id: Long`
- `nombre: String`
- `ejercitosExtra: int`
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
- `tipo: enum TipoObjetivo { COMUN, SECRETO, OCUPACION, DESTRUCCION }`
- `datosAsociados: List<String>` (ej. nombres de países o jugadores a destruir)

**Métodos:**
- `esCumplidoPor(Jugador jugador, List<Jugador> jugadores)`

---

## 🕹️ Partida

**Responsabilidad:** Gestiona el ciclo de vida del juego.

**Atributos:**
- `id: Long`
- `jugadores: List<Jugador>`
- `mapa: Mapa`
- `estado: enum EstadoPartida { INICIAL, EN_CURSO, FINALIZADA }`
- `turnoActual: int`
- `historialEventos: List<Evento>`
- `tiempoPorTurno: int` (segundos)
- `fechaUltimoMovimiento: LocalDateTime`

**Métodos:**
- `iniciarPartida()`
- `avanzarTurno()`
- `verificarGanador()`
- `guardarEstado()`
- `cargarEstado()`

---

## 🧭 Mapa

**Responsabilidad:** Representa el tablero con todos los países y continentes.

**Atributos:**
- `paises: List<Pais>`
- `continentes: List<Continente>`

**Métodos:**
- `repartirPaises(List<Jugador> jugadores)`
- `obtenerPaisPorNombre(String nombre)`
- `validarAtaque(Pais origen, Pais destino, Jugador atacante)`

---

## 🃏 TarjetaPais

**Responsabilidad:** Tarjeta que se usa para canjear por ejércitos.

**Atributos:**
- `id: Long`
- `pais: Pais`
- `simbolo: enum Simbolo { INFANTERIA, CABALLERIA, ARTILLERIA }`
- `jugadorActual: Jugador` (si está en su poder)

**Métodos:**
- `puedeCanjearseCon(TarjetaPais otra1, TarjetaPais otra2)`

---

## 📝 Evento

**Responsabilidad:** Representa un movimiento o acción dentro de la partida.

**Atributos:**
- `id: Long`
- `descripcion: String`
- `fechaHora: LocalDateTime`
- `tipo: enum TipoEvento { ATAQUE, REFUERZO, REAGRUPACION, OBJETIVO_CUMPLIDO, PARTIDA_FINALIZADA }`

**Métodos:**  
*No requiere muchos métodos, es solo para logging/historial.*

---

## 🤖 Bot (Interfaz + Estrategias)

**Responsabilidad:** Define el comportamiento de un jugador bot.

**Implementaciones:**
- `BotNovato`
- `BotBalanceado`
- `BotExperto`

---

## 🎲 Dados

**Responsabilidad:** Simula los enfrentamientos entre países durante los ataques.

**Atributos:**
- `valoresAtacante: List<Integer>` — resultados de los dados del atacante (máx. 3)
- `valoresDefensor: List<Integer>` — resultados de los dados del defensor (máx. 2)
- `perdidasAtacante: int`
- `perdidasDefensor: int`

**Métodos:**
- `lanzarDadosAtacante(int cantidad)`
- `lanzarDadosDefensor(int cantidad)`
- `resolverCombate()`
- `getResultado()`

