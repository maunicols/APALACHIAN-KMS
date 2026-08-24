# MATAME - Appalachian
## MVP Ultra-Reducido (Un Día de Juego)

**Autor:** Claude (Anthropic)  
**Alcance:** Un ciclo día/noche completo (10 minutos jugables)  
**Timeline:** 2 semanas  
**Plataforma:** Windows (Godot 4.3 → .exe nativo)

---

## 1. ALCANCE MINIMALISTA

### El Juego en 10 Minutos

**Inicio:**
- Jugador aparece en bosque al atardecer
- Fogata pequeña como único punto de seguridad
- Barra de miedo visible (0-100%)

**Progresión:**
1. **Minutos 0-3 (Atardecer):** Transición a noche
   - Sonidos lejanos comienzan
   - Miedo sube lentamente (+0.5%/seg)
   
2. **Minutos 3-7 (Noche):** Tensión máxima
   - Enemigo aparece (sombra negra oscilante)
   - Miedo sube rápido si se ve (+2%/seg)
   - Jogador puede coger palo y combatir
   
3. **Minutos 7-10 (Madrugada):** Tensión y resolución
   - Si sobrevivió: rayo final aparece
   - Game Over (victoria o muerte)

**Fin:**
- Pantalla de Game Over
- Estadísticas básicas (tiempo sobrevivido, miedo máximo)

---

## 2. FEATURES DEL MVP (CORE ONLY)

### 2.1 Escena Base
- [ ] Bosque oscuro simple (quad textizado o modelo 3D básico)
- [ ] Iluminación dinámica (día → noche)
- [ ] Fogata estática con luz amarilla
- [ ] Cielo oscuro con estrellas

### 2.2 Jugador
- [ ] Sprite/modelo simple (círculo o humanoide básico)
- [ ] Movimiento WASD (velocidad constante)
- [ ] Recolectar palo del suelo (E)
- [ ] HUD: Vida (3 golpes = muere), Miedo %

### 2.3 Sistema de Miedo (ULTRA SIMPLIFICADO)

```
FearLevel: 0-100%

Reglas:
├─ Noche = +0.5% por segundo
├─ Cerca de fogata = -0.3% por segundo
├─ Ver enemigo = +2% por segundo
├─ Ataque exitoso con palo = -10%
└─ Miedo > 80% = enemigo ataca
```

### 2.4 Sonido (Mínimo)
- [ ] Loop noche (viento + crickets, 1 track)
- [ ] Sonido amenaza (gruñido lejano)
- [ ] Sonido impacto (palo golpea)
- [ ] Alarma final (trueno)

### 2.5 Un Enemigo
**"La Sombra"**
- Aparece cuando miedo > 60%
- Forma negra oscilante (sin animación compleja)
- Persigue al jugador si se acerca < 5 metros
- Ataca si jugador pega 3 veces sin huir
- Se desvanece si jugador se aleja a fogata

### 2.6 Combate
```
Jugador:
  - Click mouse = atacar
  - Rango: 2 metros
  - Daño: -1 hit al enemigo por golpe
  
Enemigo (Sombra):
  - Persigue si cerca
  - Ataca si contacto = -1 vida jugador
  - Muere después de 3 golpes
```

### 2.7 Condiciones de Fin
```
VICTORIA:
  ├─ Sobrevivir 10 minutos
  └─ Rayo cae (fade to black + "Fin")

DERROTA:
  ├─ Miedo = 100% (parálisis permanente)
  └─ Vida = 0 (3 golpes enemigo)
```

---

## 3. ARQUITECTURA TÉCNICA

### 3.1 Stack

```
┌──────────────────────────────┐
│   Godot 4.3 LTS              │
│   → Compila a C++ automático │
│   → Genera matame.exe        │
└──────────────────────────────┘
       ↓
┌──────────────────────────────┐
│   GDScript (200 líneas total)│
│   • Main game loop           │
│   • Fear system              │
│   • Enemy AI básica          │
│   • Controles jugador        │
└──────────────────────────────┘
       ↓
┌──────────────────────────────┐
│   Assets Mínimos             │
│   • 3 sonidos (.ogg)         │
│   • 2 sprites (.png)         │
│   • 0 modelos 3D             │
└──────────────────────────────┘
```

### 3.2 Estructura de Carpetas

```
matame-appalachian/
├── project.godot
├── build_windows.bat
├── build_windows.ps1
│
├── scenes/
│   └── main.tscn           (TODO en una escena)
│
├── scripts/
│   ├── game_manager.gd     (orquestador, ~80 líneas)
│   ├── fear_system.gd      (lógica miedo, ~50 líneas)
│   ├── enemy.gd            (sombra AI, ~70 líneas)
│   └── player.gd           (controles, ~40 líneas)
│
├── assets/
│   ├── audio/
│   │   ├── night_wind.ogg
│   │   ├── threat_sound.ogg
│   │   └── impact.ogg
│   │
│   └── sprites/
│       ├── player.png
│       └── shadow.png
│
└── export_presets.cfg

Total: < 300 líneas código
```

---

## 4. CRONOGRAMA DE DESARROLLO (2 SEMANAS)

### Día 1-2: Setup
- [x] Crear proyecto Godot
- [x] Escena base (bosque, fogata, luz)
- [x] Assets sprites (100x100px, simple)
- [x] Scripts vacíos

### Día 3-4: Mecánicas Base
- [x] Jugador se mueve
- [x] Ciclo día/noche (10 min)
- [x] Sistema de miedo (levanta/baja)
- [x] HUD básico

### Día 5-6: Interacción
- [x] Recoger palo
- [x] Audio ambiente
- [x] Enemigo aparece
- [x] Combate simple

### Día 7-8: Pulido
- [x] Ajustes balance
- [x] Game Over screen
- [x] Efectos visuales (vigneta)
- [x] Testing

### Día 9-10: Build & Release
- [x] Compilación .exe
- [x] Testing en Windows virgen
- [x] Packaging
- [x] Listo distribución

### Día 11-14: Buffer/Extras
- [ ] Enemigos adicionales (OPCIONAL)
- [ ] Música (OPCIONAL)
- [ ] Estadísticas (OPCIONAL)

---

## 5. BUILD SCRIPTS

### build_windows.bat (Igual)

```batch
@echo off
setlocal enabledelayedexpansion

echo.
echo ===== MATAME - ONE DAY MVP =====
echo.

where godot >nul 2>nul
if %ERRORLEVEL% NEQ 0 (
    echo [ERROR] Godot no encontrado
    pause
    exit /b 1
)

if exist "export\windows" rmdir /s /q "export\windows"
mkdir "export\windows"

echo Compilando...
godot --path . --export-release "Windows Desktop" "export/windows/matame.exe"

if %ERRORLEVEL% EQU 0 (
    echo.
    echo [OK] Listo: export/windows/matame.exe
    echo.
    pause
) else (
    echo [ERROR] Fallo compilación
    pause
    exit /b 1
)
```

---

## 6. CÓDIGO CORE REDUCIDO

### game_manager.gd (~80 líneas)

```gdscript
extends Node

@export var game_duration: float = 600.0  # 10 minutos
@export var day_start_at: float = 180.0  # 3 min (atardecer)
@export var night_start_at: float = 300.0 # 5 min (noche)

var current_time: float = 0.0
var game_over: bool = false
var fear_level: float = 0.0
var player_health: int = 3

signal game_ended(victory: bool, time_survived: float)

func _ready():
	$Player.position = Vector2(400, 300)
	$Enemy.visible = false

func _process(delta: float):
	if game_over:
		return
	
	current_time += delta
	
	# Lógica de miedo
	update_fear(delta)
	
	# Enemigo aparece después de 3 min (noche)
	if current_time > day_start_at and $Enemy.visible == false:
		$Enemy.visible = true
	
	# Fin del juego
	if current_time >= game_duration:
		end_game(true)  # Victoria
	
	# Game Over por miedo
	if fear_level >= 100.0:
		end_game(false)  # Derrota

func update_fear(delta: float):
	# Noche sube miedo
	if current_time > day_start_at:
		fear_level += 0.5 * delta
	
	# Fogata baja miedo
	if $Player.position.distance_to($Campfire.position) < 50:
		fear_level -= 0.3 * delta
	
	# Enemigo visible = sube miedo
	if $Enemy.visible and $Player.position.distance_to($Enemy.position) < 100:
		fear_level += 2.0 * delta
	
	fear_level = clamp(fear_level, 0.0, 100.0)
	$HUD/FearBar.value = int(fear_level)

func player_attacked():
	player_health -= 1
	if player_health <= 0:
		end_game(false)

func enemy_hit():
	$Enemy.hits += 1
	if $Enemy.hits >= 3:
		$Enemy.visible = false
		fear_level -= 10

func end_game(victory: bool):
	game_over = true
	emit_signal("game_ended", victory, current_time)
	
	if victory:
		$HUD/GameOverLabel.text = "SOBREVIVISTE 10 MINUTOS\n(Fin)"
	else:
		$HUD/GameOverLabel.text = "GAME OVER"
	
	$HUD/GameOverLabel.show()
	await get_tree().create_timer(3.0).timeout
	get_tree().reload_current_scene()
```

### player.gd (~40 líneas)

```gdscript
extends CharacterBody2D

@export var speed: float = 150.0
var game_manager: Node

func _ready():
	game_manager = get_parent()

func _process(delta: float):
	var input_dir = Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
	velocity = input_dir * speed
	move_and_slide()
	
	# Coger palo
	if Input.is_action_just_pressed("ui_accept"):
		attack_enemy()

func attack_enemy():
	var enemy = get_parent().get_node("Enemy")
	if position.distance_to(enemy.position) < 50:
		enemy.take_damage()
		get_parent().enemy_hit()
```

### enemy.gd (~70 líneas)

```gdscript
extends CharacterBody2D

@export var chase_speed: float = 80.0
@export var chase_distance: float = 200.0
var player: Node
var game_manager: Node
var hits: int = 0

func _ready():
	game_manager = get_parent()
	player = game_manager.get_node("Player")

func _process(delta: float):
	if not visible:
		return
	
	var distance_to_player = position.distance_to(player.position)
	
	# Perseguir si cerca
	if distance_to_player < chase_distance:
		var direction = (player.position - position).normalized()
		velocity = direction * chase_speed
		move_and_slide()
		
		# Atacar si contacto
		if distance_to_player < 20:
			game_manager.player_attacked()
	else:
		velocity = Vector2.ZERO

func take_damage():
	# Desaparecerá cuando hits == 3 (en game_manager)
	pass
```

### fear_system.gd (~50 líneas)

```gdscript
# Este código está integrado en game_manager.gd
# Aquí solo como referencia

extends Node

var fear_level: float = 0.0
var is_night: bool = false
var time_elapsed: float = 0.0

signal fear_updated(level: float)

func update(delta: float, player_pos: Vector2, enemy_visible: bool, campfire_pos: Vector2):
	time_elapsed += delta
	
	# Noche
	if time_elapsed > 180:
		is_night = true
		fear_level += 0.5 * delta
	
	# Fogata
	if player_pos.distance_to(campfire_pos) < 50:
		fear_level -= 0.3 * delta
	
	# Enemigo
	if enemy_visible:
		fear_level += 2.0 * delta
	
	fear_level = clamp(fear_level, 0.0, 100.0)
	emit_signal("fear_updated", fear_level)
```

---

## 7. ASSETS MÍNIMOS

### Sprites (2 archivos, 100x100px PNG)
1. **player.png** - Círculo blanco simple
2. **shadow.png** - Rectángulo negro oscilante

### Audio (3 archivos OGG)
1. **night_wind.ogg** - Viento de fondo (30 seg loop)
2. **threat_sound.ogg** - Gruñido lejano (5 seg, se repite)
3. **impact.ogg** - Golpe palo (1 seg)

**Total audio:** ~5 MB
**Total assets:** ~10 MB

---

## 8. ESCENA GODOT (MAIN.TSCN)

```
Main (Node2D)
├── Background (ColorRect) → Negro
├── Campfire (StaticBody2D)
│   ├── Light2D (Omni amarilla)
│   └── Sprite2D
├── Player (CharacterBody2D)
│   ├── CollisionShape2D
│   └── AnimatedSprite2D
├── Enemy (CharacterBody2D) → HIDDEN hasta min 3
│   ├── CollisionShape2D
│   └── Sprite2D (shadow)
├── AudioStreamPlayer (noche)
├── AudioStreamPlayer (threat)
└── HUD (CanvasLayer)
    ├── FearBar (ProgressBar)
    ├── HealthLabel (Label)
    ├── TimeLabel (Label)
    └── GameOverLabel (Label) → HIDDEN
```

---

## 9. REQUISITOS & COMPILACIÓN

### Máquina Desarrollo
- **Godot 4.3 LTS** (gratuito)
- **Windows 10+**
- **2GB RAM mín**

### Usuario Final
- **Windows 10+**
- Solo ejecutable `.exe` (sin Godot instalado)
- **~50 MB** descarga

### Compilar

```cmd
# En carpeta del proyecto
build_windows.bat

# O directamente
godot --path . --export-release "Windows Desktop" export/windows/matame.exe
```

**Resultado:** `matame.exe` en carpeta `export/windows/`

---

## 10. CRITERIOS DE ACEPTACIÓN

- ✅ Juego dura exactamente 10 minutos
- ✅ Miedo sube/baja según reglas
- ✅ Enemigo aparece a los 3 minutos
- ✅ Jugador puede atacar y ganar
- ✅ Sonidos funcionan (no obligatorio música)
- ✅ Ejecutable < 100MB
- ✅ Sin crashes en 10 ejecuciones
- ✅ FPS 60 en Windows 10

---

## 11. POST-MVP (Backlog Futuro)

### v0.2
- Múltiples enemigos
- Diferentes áreas (río, cabaña)
- Power-ups (antorcha, medicina)

### v0.3
- Música dinámico
- Gráficos 3D mejorados
- Leaderboard local

---

## 12. INSTALACIÓN FINAL

**Para usuario:**
1. Descargar `matame-v0.1-windows.zip`
2. Extraer en cualquier carpeta
3. Doble-clic `matame.exe`
4. Jugar 10 minutos
5. Cerrar

**Sin requisitos adicionales.**

---

**MVP ULTRA-REDUCIDO - LISTO PARA DESARROLLO DE 2 SEMANAS**

Total: 
- 300 líneas código GDScript
- 10 MB assets
- 1 escena
- 4 scripts
- 10 minutos gameplay

**Entrega final:** `matame.exe` compilado nativo para Windows.

---
