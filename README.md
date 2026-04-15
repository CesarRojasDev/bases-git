# Comandos Básicos de Git

## Configuración inicial

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

---

## Crear / Clonar repositorios

```bash
git init                        # Inicializar repo en carpeta actual
git clone <url>                 # Clonar repositorio remoto
```

---

## Estado y seguimiento

```bash
git status                      # Ver estado de archivos
git log                         # Ver historial de commits
git log --oneline               # Historial resumido
git diff                        # Ver cambios no preparados
```

---

## Staging y commits

```bash
git add <archivo>               # Agregar archivo al staging
git add .                       # Agregar todos los cambios
git commit -m "mensaje"         # Crear commit
git commit -am "mensaje"        # Add + commit (archivos ya rastreados)
```

---

## Ramas (branches)

```bash
git branch                      # Listar ramas
git branch <nombre>             # Crear rama
git checkout <rama>             # Cambiar de rama
git checkout -b <rama>          # Crear y cambiar de rama
git switch <rama>               # (alternativa moderna)
git merge <rama>                # Fusionar rama a la actual
git branch -d <rama>            # Eliminar rama
```

---

## Repositorios remotos

```bash
git remote -v                   # Ver remotos configurados
git remote add origin <url>     # Agregar remoto
git push origin <rama>          # Enviar cambios
git pull origin <rama>          # Traer y fusionar cambios
git fetch                       # Traer cambios sin fusionar
```

---

## Deshacer cambios

```bash
git restore <archivo>           # Descartar cambios de un archivo
git restore .                   # Descartar TODOS los cambios en working dir
git restore --staged <archivo>  # Sacar un archivo del staging
git restore --staged .          # Sacar TODOS los archivos del staging
git reset HEAD~1                # Deshacer último commit (guarda cambios)
git reset --hard HEAD~1         # Deshacer último commit (elimina cambios)
git revert <hash>               # Crear commit que revierte otro
```

---

## Otros útiles

```bash
git stash                       # Guardar cambios temporalmente
git stash pop                   # Recuperar cambios guardados
git tag v1.0                    # Crear etiqueta
git cherry-pick <hash>          # Aplicar commit específico
```

---

## Flujo típico del día a día

```
pull → hacer cambios → add → commit → push
```

---

## Flujo de trabajo en equipo con ramas

### Estructura de ramas

```
main          ← producción, siempre estable
desarrollo    ← integración, aquí se juntan todos
├── dev-cesar
├── dev-luciana
└── dev-antonio
```

---

### Flujo completo — ejemplo con dev-cesar

**1. Antes de empezar, actualizar desarrollo**
```bash
git checkout desarrollo
git pull origin desarrollo
```

**2. Crear o actualizar tu rama personal**
```bash
git checkout dev-cesar
git merge desarrollo          # Traer lo nuevo de desarrollo a tu rama
```

**3. Trabajar y subir tus cambios**
```bash
git add .
git commit -m "feat: nueva funcionalidad X"
git push origin dev-cesar
```

**4. Fusionar tu rama a desarrollo (merge)**
```bash
git checkout desarrollo
git pull origin desarrollo    # Asegurarse que está al día
git merge dev-cesar           # Fusionar tus cambios
git push origin desarrollo
```

**5. Cuando desarrollo está listo para producción**
```bash
git checkout main
git pull origin main
git merge desarrollo
git push origin main
```

---

### Traer cambios de desarrollo a tu rama (sync)

Cuando otros (Luciana, Antonio) ya subieron cambios a `desarrollo` y necesitas tenerlos:

```bash
git checkout desarrollo
git pull origin desarrollo

git checkout dev-cesar
git merge desarrollo          # Trae los cambios nuevos a tu rama
```

O con rebase (historial más limpio):
```bash
git checkout dev-cesar
git rebase desarrollo
```

---

### Resolver conflictos al hacer merge

Si Git detecta conflictos:
```bash
# 1. Git marca los archivos con conflicto
git status                    # Ver qué archivos tienen conflicto

# 2. Abrir el archivo y resolver manualmente
# Buscar los marcadores:
<<<<<<< dev-cesar
  tu código
=======
  código de desarrollo
>>>>>>> desarrollo

# 3. Después de resolver
git add <archivo>
git commit -m "fix: resolver conflicto merge desarrollo"
```

---

### Resumen visual del flujo

```
dev-cesar    →─── merge ──→ desarrollo →─── merge ──→ main
dev-luciana  →─── merge ──→
dev-antonio  →─── merge ──→
```
