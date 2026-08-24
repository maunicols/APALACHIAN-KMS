# Instrucciones de Setup GitHub

Este documento contiene los comandos exactos para pushear el setup inicial a tu repo GitHub.

---

## ✅ Pasos Previos

Asegúrate de tener:
1. Git instalado (`git --version` para verificar)
2. Credenciales de GitHub configuradas
3. Tu repositorio ya creado: `maunicols/APALACHIAN-KMS`

---

## 🚀 Pushear Setup Inicial

### Opción A: Clonar y Reemplazar (RECOMENDADO)

```bash
# 1. Ve a la carpeta donde quieres el proyecto
cd C:\Proyectos

# 2. Clona tu repositorio vacío
git clone https://github.com/maunicols/APALACHIAN-KMS.git

# 3. Entra en la carpeta
cd APALACHIAN-KMS

# 4. Copia TODOS los archivos de matame-setup a esta carpeta
# (O manualmente mueve los archivos en el Explorador)

# 5. Configura git si es primera vez
git config user.name "Tu Nombre"
git config user.email "tu@email.com"

# 6. Verifica qué va a cambiar
git status

# 7. Agrega todo
git add .

# 8. Commit inicial
git commit -m "feat: Initial project setup - Godot 4.3 structure and scripts"

# 9. Push a main
git push -u origin main
```

---

### Opción B: Desde la Terminal (Si Git está en PATH)

```powershell
# PowerShell

# Navegar a tu repo
cd C:\Ruta\A\APALACHIAN-KMS

# Copiar contenido (Windows PowerShell)
Copy-Item "C:\Ruta\A\matame-setup\*" -Destination . -Recurse

# Agregar y pusher
git add .
git commit -m "feat: Initial project setup - Godot 4.3 structure and scripts"
git push origin main
```

---

### Opción C: Directo desde CMD (Windows)

```batch
cd C:\Ruta\A\APALACHIAN-KMS

REM Copiar todos los archivos
xcopy "C:\Ruta\A\matame-setup\*" . /E /I

REM Git
git add .
git commit -m "feat: Initial project setup - Godot 4.3 structure and scripts"
git push origin main
```

---

## ✅ Verificar que funcionó

Abre GitHub en el navegador:
```
https://github.com/maunicols/APALACHIAN-KMS
```

Deberías ver:
- ✅ Carpeta `scripts/` con 4 archivos `.gd`
- ✅ Carpeta `scenes/` (vacía)
- ✅ Carpeta `assets/` con subcarpetas (vacías)
- ✅ Archivos: `project.godot`, `build_windows.bat`, `README.md`
- ✅ Commit con mensaje "feat: Initial project setup..."

---

## 📝 Próximos Commits (Desarrollo)

Después de pushear el setup, cada cambio se pushea así:

```bash
# Editar archivos en Godot...

# Cuando termines un feature
git add .
git commit -m "feat: Implement X mechanic"
git push origin main

# O si trabajas en develop
git push origin develop
```

---

## 🔀 Crear rama develop (OPCIONAL)

Si quieres una rama separada para desarrollo:

```bash
git checkout -b develop
git push -u origin develop

# Luego siempre haces commit en develop
git add .
git commit -m "..."
git push origin develop
```

---

## ❓ Problemas Comunes

**Error: "fatal: not a git repository"**
```bash
# Asegúrate de estar en la carpeta correcta
cd APALACHIAN-KMS
git status  # Debería funcionar
```

**Error: "permission denied"**
```bash
# Genera SSH key o usa HTTPS token
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

**Error: "branch not found"**
```bash
# Asegúrate de que la rama existe
git branch -a  # Listar todas las ramas
git push -u origin main  # Crear y pusher
```

---

## 🎯 Resumen

Una vez hayas ejecutado los pasos:

1. Tu repo en GitHub tendrá toda la estructura
2. Puedes abrir Godot y empezar a desarrollar
3. Cada cambio se pushea con `git add . && git commit -m "..." && git push`

¡Listo para empezar! 🚀

---

**Fecha:** 2026-08-24  
**Proyecto:** MATAME - Appalachian MVP
