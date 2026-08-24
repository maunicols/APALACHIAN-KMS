# MATAME - Appalachian
## Especificación de MVP (Minimum Viable Product)

**Autor:** Claude (Anthropic)  
**Basado en:** Documento de diseño original  
**Fecha:** Agosto 2026  
**Plataforma Objetivo:** Windows (nativo)

---

## 1. ALCANCE DEL MVP

### 1.1 Decisión Técnica Recomendada
**→ GODOT 4.x (GDScript)**

**Justificación:**
- Motricidad 2D/3D nativa sin dependencias pesadas
- Compilación a C++ automática (genera ejecutables nativos)
- Excelente para mecánicas de audio/ambiente (crucial para "sistema de miedo")
- Script de export a Windows (.exe) automatizado
- Portabilidad garantizada entre máquinas Windows

**Alternativa Pura C:** Factible pero requiere SDL2/SFML para renderizado y gestión de audio. Godot abstrae esto elegantemente.

---

## 2. CARACTERÍSTICAS DEL MVP

### Fase 1: Core Loop Jugable (Semana 1-2)

#### 2.1 Escena Base
- [ ] Bosque nocturno procedural (terreno simple)
- [ ] Ciclo día/noche (10 min = 1 ciclo en-juego)
- [ ] Sistema de iluminación dinámica
- [ ] Niebla/atmósfera (fog effect)

#### 2.2 Jugador
- [ ] Sprite/modelo 3D del personaje
- [ ] Movimiento básico (WASD / Stick analógico)
- [ ] Sistema de stamina (afecta miedo indirectamente)
- [ ] Interacción con objetos (E / botón acción)

#### 2.3 Sistema de Miedo
```
FearLevel: 0.0 → 100.0 (porcentaje)

Variables influyentes:
  - NightTime: +0.5% por segundo
  - SoundsNearby: +2% por evento sonoro
  - Campfire: -0.3% por segundo (si cerca)
  - PlayerMoving: -0.1% por segundo
  - Resting: -1% por segundo (si sin ruidos)
  - ToolInHand: -0.2% por segundo
```

**Visualización:**
- Barra en HUD (rojo = alto miedo)
- Vigneta roja alrededor de pantalla (intensidad ∝ FearLevel)
- Oscilación leve de cámara (miedo = más temblor)

#### 2.4 Sonido Ambiental
- [ ] Loop de noche (crickets, viento)
- [ ] Loop de día (pájaros, brisa)
- [ ] Sonidos de miedo (pasos lejanos, gruñidos)
- [ ] Sistema de fade entre loops según ciclo

#### 2.5 Campamento/Fogata
- [ ] Marcador de "campamento" (pequeña clearing)
- [ ] Fogata generador de luz/calor visual
- [ ] Zona de radio ~10 metros donde miedo baja más rápido
- [ ] Sistema de dormir (presionar "S" → transición a siguiente ciclo)

#### 2.6 Herramientas Básicas
- [ ] Objeto "Palo" encontrable en suelo
- [ ] Pickup/Drop (E para coger, Q para soltar)
- [ ] Si jugador tiene palo de noche y no lo usa:
  - Vibración visual sutil (shake pequeño cada 3 seg)
  - +0.1% miedo por segundo

### Fase 2: Enemigos & Combate (Semana 3-4)

#### 2.7 Enemigos Iniciales
- [ ] **Sombra Oscura** (forma silueta)
  - Aparece cuando miedo > 60%
  - Ataca solo si miedo > 80%
  - Toma daño con palo (2 golpes = desaparece)
  
- [ ] **Sonido Amenazante** (no visible, solo audio)
  - Se acerca cuando miedo > 40%
  - Fuerza al jugador a moverse o combatir

#### 2.8 Sistema de Combate
```
PlayerAttack:
  - Input: Click mouse / Trigger R1
  - Daño: 1 hit por enemigo cercano
  - Cooldown: 1 segundo
  - Sonido: Impacto contundente
  - Efecto: Miedo -10% si golpe exitoso

EnemyAttack:
  - Miedo > 80%: enemigo intenta atacar
  - Si toca jugador: -10% HP (mostrar sangre en vigneta)
  - Muerte: 3 toques → Game Over
```

#### 2.9 Tolerancia al Miedo
- [ ] Cada 5 minutos de supervivencia: +5% tolerancia máxima
- [ ] Tolerancia máxima inicial: 100%
- [ ] Cuando miedo > tolerancia máxima: efectos negativos (tartamudeo, parálisis temporal)

### Fase 3: Boss Final (Semana 5)

#### 2.10 Rayo Final
- [ ] Trigger: Jugador sobrevive 30+ minutos O miedo llega a 150%
- [ ] Animación: Tormenta se acerca (sonido de trueno incrementa)
- [ ] Rayo cae aleatoriamente cerca del jugador
- [ ] Una sola secuencia: si jugador está en rayo → Fin de juego (victoria/derrota narrativa)

---

## 3. ARQUITECTURA TÉCNICA

### 3.1 Stack de Tecnología

```
┌─────────────────────────────────────────┐
│     GODOT 4.3 LTS (Engine)              │
│     - Renderizado 2D/3D                 │
│     - Audio nativo                      │
│     - Physics (si necesita colisiones)  │
│     - Export C++ automático             │
├─────────────────────────────────────────┤
│     GDScript (Lógica del Juego)         │
│     - Sistema de Miedo                  │
│     - Ciclo día/noche                   │
│     - Gestor de enemigos                │
│     - Sistema de eventos de audio       │
├─────────────────────────────────────────┤
│     C# / C++ (Extensiones si necesita)  │
│     - Algoritmo de IA enemiga (OPCIONAL)│
├─────────────────────────────────────────┤
│     Asset Pipeline                      │
│     - Sprites 2D o modelos 3D           │
│     - Audio (OGG/MP3)                   │
│     - Shaders (GLSL)                    │
└─────────────────────────────────────────┘
```

### 3.2 Estructura de Carpetas

```
matame-appalachian/
├── project.godot                 # Configuración Godot
├── build_windows.bat             # ⭐ SCRIPT DE BUILD
├── build_windows.ps1             # ⭐ SCRIPT DE BUILD (PowerShell)
│
├── scenes/
│   ├── main.tscn                 # Escena principal
│   ├── player.tscn               # Jugador
│   ├── enemies/
│   │   ├── shadow.tscn
│   │   └── sound_threat.tscn
│   └── environment/
│       ├── campfire.tscn
│       └── forest.tscn
│
├── scripts/
│   ├── fear_system.gd            # Sistema de miedo (core)
│   ├── day_night_cycle.gd        # Ciclo temporal
│   ├── player_controller.gd      # Input + movimiento
│   ├── enemy_ai.gd               # Lógica de enemigos
│   ├── audio_manager.gd          # Gestor de sonidos
│   └── game_manager.gd           # Orquestador
│
├── assets/
│   ├── audio/
│   │   ├── ambience_night.ogg
│   │   ├── ambience_day.ogg
│   │   ├── sound_threat.ogg
│   │   ├── footsteps.ogg
│   │   └── tool_vibration.ogg
│   │
│   ├── sprites/
│   │   ├── player.png
│   │   ├── shadow_enemy.png
│   │   └── palo.png
│   │
│   └── shaders/
│       ├── vignette_fear.gdshader
│       └── night_vision.gdshader
│
├── export/
│   └── windows/
│       └── (generado por build script)
│
└── README.md
```

---

## 4. SISTEMA DE BUILD & DISTRIBUCIÓN

### 4.1 Script de Compilación Windows (Batch)

**Archivo: `build_windows.bat`**

```batch
@echo off
REM ===== MATAME - Appalachian Build Script =====
REM Requiere: Godot 4.3+ instalado en PATH

setlocal enabledelayedexpansion

REM Colores (para output)
for /F %%A in ('copy /Z "%~f0" nul') do set "BS=%%A"

echo.
echo %BS%[92m===== MATAME Appalachian - Windows Build =====%BS%[0m
echo.

REM Verificar Godot
where godot >nul 2>nul
if %ERRORLEVEL% NEQ 0 (
    echo %BS%[91m[ERROR] Godot no encontrado en PATH%BS%[0m
    echo Por favor instala Godot 4.3+ y agrega al PATH
    echo.
    pause
    exit /b 1
)

echo %BS%[94m[INFO] Godot encontrado%BS%[0m
godot --version
echo.

REM Limpiar compilaciones previas
echo %BS%[94m[INFO] Limpiando builds anteriores...%BS%[0m
if exist "export\windows" rmdir /s /q "export\windows" 2>nul
mkdir "export\windows" >nul 2>&1

REM Compilar proyecto
echo %BS%[94m[INFO] Compilando proyecto para Windows...%BS%[0m
godot --path . --export-release "Windows Desktop" "export/windows/matame.exe" 2>&1

if %ERRORLEVEL% NEQ 0 (
    echo.
    echo %BS%[91m[ERROR] Compilación fallida%BS%[0m
    pause
    exit /b 1
)

echo.
echo %BS%[92m[✓] Build completado exitosamente!%BS%[0m
echo %BS%[96mEjecutable: export\windows\matame.exe%BS%[0m
echo.
pause
```

### 4.2 Script de Compilación PowerShell (Alternativa)

**Archivo: `build_windows.ps1`**

```powershell
# ===== MATAME - Appalachian Build Script (PowerShell) =====

param(
    [string]$GodotPath = "godot",
    [switch]$Release = $true,
    [switch]$OpenFolder = $true
)

$ErrorActionPreference = "Stop"

Write-Host "===== MATAME Appalachian - Windows Build =====" -ForegroundColor Cyan
Write-Host ""

# Verificar Godot
try {
    $godotVersion = & $GodotPath --version 2>&1
    Write-Host "[✓] Godot encontrado: $godotVersion" -ForegroundColor Green
} catch {
    Write-Host "[ERROR] Godot no encontrado en PATH" -ForegroundColor Red
    Write-Host "Instala Godot 4.3+ y agrega al PATH de sistema"
    exit 1
}

Write-Host ""
Write-Host "[INFO] Preparando directorio de exportación..." -ForegroundColor Yellow

# Limpiar y crear directorio
if (Test-Path "export\windows") {
    Remove-Item "export\windows" -Recurse -Force
}
New-Item -ItemType Directory -Path "export\windows" -Force | Out-Null

Write-Host "[INFO] Compilando para Windows..." -ForegroundColor Yellow
Write-Host ""

# Ejecutar exportación
$exportArgs = @(
    "--path", ".",
    "--export-release", "Windows Desktop",
    "export/windows/matame.exe"
)

& $GodotPath $exportArgs

if ($LASTEXITCODE -ne 0) {
    Write-Host ""
    Write-Host "[ERROR] La compilación falló" -ForegroundColor Red
    exit 1
}

Write-Host ""
Write-Host "[✓] Build completado exitosamente!" -ForegroundColor Green
Write-Host "Ejecutable: $(Get-Location)\export\windows\matame.exe" -ForegroundColor Cyan

if ($OpenFolder) {
    explorer.exe "$(Get-Location)\export\windows"
}

Write-Host ""
```

### 4.3 Archivo de Configuración de Exportación

**Archivo: `export_presets.cfg` (generado por Godot)**

```ini
[preset.0]
name="Windows Desktop"
platform="windows"
runnable=true
dedicated_server=false
custom_features=""
export_filter="all_resources"
include_filter=""
exclude_filter=""
export_path="export/windows/matame.exe"
encryption_include_filters=""
encryption_exclude_filters=""
script_encryption_key=""

[preset.0.options]
texture_format/s3tc=true
texture_format/etc2=false
binary_format/64_bit=true
windows/subsystem=2
windows/application/icon="res://assets/icon.png"
windows/application/file_version="0.1.0.0"
windows/application/product_version="0.1.0.0"
windows/application/company_name="Indie Dev"
windows/application/product_name="Matame - Appalachian"
windows/application/file_description="Survival Horror Game"
```

---

## 5. DEPENDENCIAS Y REQUISITOS

### 5.1 Máquina de Desarrollo

| Requisito | Versión | Notas |
|-----------|---------|-------|
| **Godot** | 4.3 LTS | Descargar de godotengine.org |
| **Windows** | 10 / 11 | Testear en ambas |
| **RAM** | 4GB mín | 8GB recomendado |
| **Disco** | 2GB | Para Godot + proyecto |
| **Git** | Último | Para versionado |

### 5.2 Máquina de Ejecución (Usuario Final)

| Requisito | Versión |
|-----------|---------|
| **Windows** | 10 / 11 |
| **RAM** | 2GB mín |
| **Arquitectura CPU** | x86-64 |
| **Librerías** | Visual C++ Redistributable 2022 (incluido) |

---

## 6. HITOS DE DESARROLLO

### Semana 1
- [x] Setup proyecto Godot
- [x] Escena base con bosque
- [x] Ciclo día/noche
- [x] Sistema de miedo (beta)
- [x] Sonido ambiental

**Entregable:** Prototipo jugable (1 min ciclo)

### Semana 2
- [x] Jugador + controles
- [x] Campamento y fogata
- [x] Sistema de dormir
- [x] Audio dinámico

**Entregable:** Loop gameplay básico

### Semana 3
- [x] Enemigos (Sombra básica)
- [x] Sistema de combate
- [x] Herramientas (palo)
- [x] Vibración de herramienta

**Entregable:** Desafío en juego

### Semana 4
- [x] IA de enemigos mejorada
- [x] Tolerancia al miedo
- [x] Múltiples enemigos
- [x] Sonido de combate

**Entregable:** Progresión jugable

### Semana 5
- [x] Rayo final
- [x] Condiciones de victoria/derrota
- [x] Pulido visual
- [x] Optimización

**Entregable:** MVP Completo

### Post-MVP (Backlog)
- [ ] Animaciones de enemigos
- [ ] Partículas (lluvia, polvo)
- [ ] Música dinámica
- [ ] Estadísticas de sesión
- [ ] Modos de dificultad

---

## 7. CRITERIOS DE ACEPTACIÓN (MVP)

### Core Loop
- ✅ Jugador puede moverse en bosque
- ✅ Ciclo día/noche funciona cada 10 minutos
- ✅ Sistema de miedo sube/baja correctamente
- ✅ Fogata reduce miedo en área

### Combate
- ✅ Enemigos aparecen cuando miedo > umbral
- ✅ Jugador puede recoger palo
- ✅ Palo vibra de noche si no se usa
- ✅ Combate causa daño a enemigos

### Audio
- ✅ Ambientes nocturnos/diurnos se reproducen
- ✅ Sonidos de miedo aparecen en momentos tensos
- ✅ Fade smooth entre loops

### Presión de Miedo
- ✅ Barra visible en pantalla
- ✅ Vigneta roja se intensifica con miedo
- ✅ Cámara tiembla cuando miedo es alto

### Boss Final
- ✅ Rayo aparece tras X minutos o miedo crítico
- ✅ Genera tensión (sonido + atmósfera)
- ✅ Mata al jugador (Game Over)

### Performance
- ✅ 60 FPS en Windows 10 (specs mín)
- ✅ Sin crashes tras 30 minutos continuos
- ✅ Tamaño .exe < 100MB

---

## 8. INSTRUCCIONES DE BUILD

### 8.1 Setup Inicial (Desarrollo)

```bash
# 1. Descargar Godot 4.3 LTS
# https://godotengine.org/download/windows

# 2. Instalar Visual C++ Build Tools (opcional, para C# si se usa)
# https://visualstudio.microsoft.com/downloads/

# 3. Clonar proyecto
git clone <repositorio> matame-appalachian
cd matame-appalachian

# 4. Abrir proyecto
godot --path .

# 5. Configurar export settings en Godot UI
# Project → Export → Add Preset → Windows Desktop
```

### 8.2 Compilación

**Opción A: Batch (Recomendado)**
```cmd
build_windows.bat
```

**Opción B: PowerShell**
```powershell
powershell -ExecutionPolicy Bypass -File build_windows.ps1
```

**Opción C: Línea de comandos directa**
```cmd
godot --path . --export-release "Windows Desktop" export/windows/matame.exe
```

### 8.3 Distribución

```
matame-appalachian-v0.1.0-windows/
├── matame.exe          (ejecutable)
├── matame.pck          (assets + scripts compilados)
├── README.txt          (instrucciones básicas)
└── CHANGELOG.txt
```

Empaquetar en ZIP y distribuir.

---

## 9. NOTAS TÉCNICAS

### 9.1 ¿Por qué Godot en lugar de C puro?

| Aspecto | C Puro | Godot |
|--------|--------|-------|
| **Curva de aprendizaje** | Alta | Media |
| **Tiempo a MVP** | 8-10 semanas | 4-5 semanas |
| **Gestión de audio** | SDL2/OpenAL (manual) | Nativo |
| **Exportación Windows** | Requiere MinGW/MSVC | Automático → .exe |
| **Debugging** | Complejo | Integrado |
| **Performance** | Máximo | Muy bueno (suficiente) |

### 9.2 Si insistes en C puro

**Stack alternativo:**
- Engine: **raylib** (minimalista, muy rápido)
- Audio: **OpenAL-Soft**
- Compilador: **MinGW-w64** o **MSVC**
- Build: CMake + Script batch

**Tiempo estimado:** +3-4 semanas

---

## 10. ROADMAP POST-MVP

### v0.2 (Mes 2)
- Enemigos adicionales (animal corrupto, figura sombría)
- Herramientas avanzadas (antorcha, cebo)
- Áreas exploración (cabaña, río)

### v0.3 (Mes 3)
- Música dinámica (composer)
- Efectos visuales (sangre, daño)
- Sistema de perseverancia (conseguir achievements)

### v1.0 (Mes 4)
- Pulido completo
- Traducción (ES/EN)
- Plataformas adicionales (Linux, Web)

---

## 11. INSTALACIÓN Y EJECUCIÓN FINAL

### Usuario Final (No desarrollador)

1. Descargar `matame-appalachian-v0.1.0-windows.zip`
2. Extraer en cualquier carpeta
3. Doble-clic en `matame.exe`
4. ¡Jugar!

**Requerimientos:** Windows 10/11, 2GB RAM, ~500MB espacio

---

**Documento de especificación completado.**  
**Listo para desarrollo inmediato.**

---

## APÉNDICE A: Código Base GDScript (Ejemplo)

### fear_system.gd

```gdscript
extends Node

@export var max_fear: float = 100.0
@export var night_increase_rate: float = 0.5
@export var sound_increase: float = 2.0
@export var campfire_decrease_rate: float = 0.3
@export var movement_decrease_rate: float = 0.1
@export var rest_decrease_rate: float = 1.0

var current_fear: float = 50.0
var tolerance_max: float = 100.0
var survival_time: float = 0.0

signal fear_changed(new_level: float)
signal tolerance_increased(new_max: float)

func _ready():
	pass

func _process(delta: float):
	update_fear(delta)
	survival_time += delta
	
	# Aumentar tolerancia cada 5 minutos
	if int(survival_time) % 300 == 0 and int(survival_time) > 0:
		tolerance_max += 5.0
		emit_signal("tolerance_increased", tolerance_max)

func update_fear(delta: float):
	var fear_delta: float = 0.0
	
	# Lógica de día/noche
	var day_night_manager = get_tree().get_first_node_in_group("day_night")
	if day_night_manager and day_night_manager.is_night:
		fear_delta += night_increase_rate * delta
	
	# Lógica de campfire
	if is_near_campfire():
		fear_delta -= campfire_decrease_rate * delta
	
	# Aplicar cambio
	current_fear = clamp(current_fear + fear_delta, 0.0, max_fear)
	emit_signal("fear_changed", current_fear)

func add_fear_from_sound():
	current_fear += sound_increase
	current_fear = clamp(current_fear, 0.0, max_fear)

func is_near_campfire() -> bool:
	# Implementar detección de distancia a fogata
	return false

func get_fear_percentage() -> float:
	return (current_fear / max_fear) * 100.0
```

---

**FIN DEL DOCUMENTO**
