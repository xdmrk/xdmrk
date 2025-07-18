# Guía de Git y GitHub

**Documentación de comandos esenciales y flujos de trabajo con explicaciones prácticas**

## Índice
1. [Configuración Inicial](#configuración-básica) - Primeros pasos con Git
2. [Gestión de Repositorios](#gestión-de-repositorios) - Iniciar y clonar proyectos
3. [Seguimiento de Cambios](#seguimiento-de-cambios) - Add, Commit, Status
4. [Trabajo con Ramas](#trabajo-con-ramas) - Branching y Merging
5. [Repositorios Remotos](#repositorios-remotos) - Push, Pull, Fetch
6. [Historial y Versiones](#historial-y-versiones) - Log, Diff, Checkout
7. [Fusión de Proyectos](#fusión-de-proyectos) - Unir repositorios
8. [Resolución de Problemas](#resolución-de-problemas) - Conflictos y errores comunes
9. [Flujos Recomendados](#flujos-recomendados) - Buenas prácticas
10. [Alias Útiles](#alias-útiles) - Atajos para productividad


---

## Configuración Básica
Configuración inicial necesaria antes de usar Git
```bash
# Establece tu nombre para los commits (visible en GitHub)
git config --global user.name "Tu Nombre"

# Configura tu email (debe coincidir con GitHub)
git config --global user.email tu@email.com

# Activa colores en la terminal para mejor visualización
git config --global color.ui true
# Personalizar colores específicos (ej. para branch en log)
git config --global color.branch.current "yellow reverse"
git config --global color.status.added "green bold"

# Establece VS Code como editor por defecto
git config --global core.editor "code --wait"

# Ver todas las configuraciones actuales
git config --global --list
```


## Gestión de Repositorios
Comandos para iniciar y clonar proyectos
```bash
# Inicializa un nuevo repositorio Git en la carpeta actual
# (Crea un .git/ oculto con toda la base de datos)
git init

# Clona un repositorio remoto (trae toda la historia y ramas)
git clone https://github.com/usuario/repo.git

# Clona solo una rama específica (útil para repos grandes)
git clone -b desarrollo https://github.com/usuario/repo.git
```

Comandos si ya tienes una carpeta con tus archivos listos para convertirla en repositorio
```bash
# Abre la terminal y navega a tu carpeta
cd ruta/de/tu/carpeta

# Inicializa el repositorio Git
git init

# Verifica el estado inicial (verás todos tus archivos listados como "Untracked" (no seguidos por Git aún).)
git status

# Añade todos los archivos al staging (El punto significa "todos los archivos en esta carpeta")
git add .

# Haz tu primer commit
git commit -m "Initial commit: proyecto listo para control de versiones"

# (Opcional) Conecta con un repositorio remoto (GitHub/GitLab)
git remote add origin https://github.com/tu-usuario/tu-repo.git
git push -u origin main
```
Si no conectas tu repositorio local con un remoto (GitHub/GitLab/Bitbucket): Guarda historial completo de cambios (git log); puedes crear ramas, hacer merges y revertir cambios; sigues teniendo control de versiones (pero solo local).


## Seguimiento de Cambios
Cómo registrar modificaciones en tu proyecto
```bash
# Muestra el estado actual: archivos modificados, staged y no trackeados
git status

# Añade TODOS los archivos modificados al área de staging
git add .

# Añade solo un archivo específico
git add archivo.html

# Hace commit de los cambios con un mensaje descriptivo
# (Los cambios quedan registrados permanentemente en el historial)
git commit -m "Implementa función de login"

# Corrige el último commit (modifica el mensaje o añade archivos olvidados)
git commit --amend -m "Nuevo mensaje más descriptivo"
```


## Trabajo con Ramas
Estrategias para organizar el desarrollo
```bash
# Lista todas las ramas locales (* muestra la rama actual)
git branch

# Crea una nueva rama basada en la actual
git branch nueva-funcionalidad

# Cambia a otra rama (actualiza archivos en tu directorio)
git checkout desarrollo

# Crea y cambia a una nueva rama en un solo paso
git checkout -b hotfix

# Fusiona una rama con la actual (importante: estar en la rama destino)
git merge nueva-funcionalidad

# Elimina una rama local (asegúrate de haberla mergeado)
git branch -d rama-vieja
```


## Repositorios Remotos
Trabajo con GitHub/GitLab/Bitbucket
```bash
# Conecta tu repo local con un remoto (origin es el nombre convencional)
git remote add origin https://github.com/usuario/repo.git

# Sube cambios al remoto por primera vez (-u guarda la relación)
git push -u origin main

# Sube una rama específica al remoto
git push origin desarrollo

# Descarga cambios del remoto y fusiona automáticamente
git pull origin main

# Trae cambios del remoto SIN fusionar (permite revisar antes)
git fetch origin
```


## Historial y Versiones
Cómo navegar por el historial del proyecto
```bash
# Muestra commits ordenados por fecha (q para salir)
git log

# Versión compacta del historial (ideal para ramas complejas)
git log --oneline --graph --all

# Compara cambios entre dos commits (hash de 7 caracteres)
git diff abc1234 xyz5678

# Revierte a un commit específico (cuidado: pierdes cambios posteriores)
git checkout 3a5b7c9

# Crea un tag para versiones importantes (v1.0, v2.1, etc.)
git tag -a v1.2 -m "Versión estable con todas las features"
```


## Fusión de Proyectos
Cómo combinar repositorios separados
```bash
# Método 1: Conservando todo el historial
mkdir proyecto-unificado
cd proyecto-unificado
git init
git remote add repo1 https://github.com/usuario/repo1.git
git fetch repo1
git merge repo1/main --allow-unrelated-histories

# Método 2: Partiendo de cero (solo archivos actuales)
cd carpeta-organizada
git init
git add .
git commit -m "Proyecto unificado inicial"
git remote add origin https://github.com/usuario/learning.git
git push -u origin main
```
No se creará una carpeta repo1/ o repo2/ dentro de nuevoRepo y o se perderán los historiales de commits (puedes verlos con `git log --all`).

**Recomendaciones**
1. Haz backup de los repos originales antes de fusionar.
2. Usa `git status` frecuentemente para ver archivos duplicados/conflictos.
3. Si prefieres mantener separados los contenidos, usa `git subtree` (más avanzado).
   ```bash
   # Alternativa con subtree (para mantener estructuras)
   git subtree add --prefix=repo1/ <URL_repo1> main --squash
   git subtree add --prefix=repo2/ <URL_repo2> main --squash
 	```


## Resolución de Problemas
Soluciones para situaciones comunes
```bash
# Deshacer cambios locales no commitados (¡Cuidado! Irreversible)
git reset --hard HEAD

# Eliminar archivos no deseados del tracking
git rm --cached archivo-secreto.txt

# Recuperar archivos eliminados accidentalmente
git checkout HEAD -- archivo-importante.js

# Cuando git pull falla por conflictos:
git fetch origin
git merge --abort  # Cancela el merge conflictivo
git mergetool      # Usa herramienta visual para resolver
```


## Flujos Recomendados
Buenas prácticas para trabajo eficiente
1. Flujo básico diario:
```bash
git checkout main
git pull origin main
git checkout -b nueva-feature
# Hacer cambios...
git add .
git commit -m "Implementa nueva funcionalidad"
git push origin nueva-feature
# Crear Pull Request en GitHub
```
2. Hotfix para producción:
```bash
git checkout main
git pull origin main
git checkout -b hotfix/error-login
# Corregir el error
git add .
git commit -m "Corrige fallo en sistema de login"
git push origin hotfix/error-login
```


## Alias Útiles
Atajos para mejorar tu productividad

Agregar al archivo `~/.bashrc` o `~/.zshrc`:
```bash
alias gs='git status'
alias ga='git add'
alias gaa='git add --all'
alias gc='git commit -m'
alias gcm='git checkout main'
alias gl='git log --oneline --graph --all'
alias gd='git diff'
alias gpl='git pull origin main'
alias gps='git push origin main'
```

Para aplicar los cambios:
```bash
source ~/.bashrc
```

## Consejos Finales
1. Commits atómicos: Cada commit debe representar un cambio lógico único
2. Mensajes claros: Usa el formato: "Tipo: Descripción" (ej: "Feat: Add dark mode")
3. .gitignore: Siempre excluye archivos generados (node_modules/, .env, etc.)
4. Pull antes de Push: Evita conflictos sincronizando primero
5. Branches descriptivos: Usa nombres como feature/auth o fix/navbar
```bash
# Plantilla .gitignore básica para proyectos web
node_modules/
.env
.DS_Store
dist/
*.log
```






<!--
## Configuración Básica

Configurar Nombre que salen en los commits
```ssh
	git config --global user.name "dasdo"
```
Configurar Email
```ssh	
	git config --global user.email dasdo1@gmail.com
```
Marco de colores para los comando
```ssh
	git config --global color.ui true
```

## Iniciando repositorio

Iniciamos GIT en la carpeta donde esta el proyecto
```ssh
	git init
```
Clonamos el repositorio de github o bitbucket
```ssh
	git clone <url>
```
Añadimos todos los archivos para el commit
```ssh
	git add .
```
Hacemos el primer commit
```ssh
	git commit -m "Texto que identifique por que se hizo el commit"
```
subimos al repositorio
```ssh
	git push origin master
```

## GIT CLONE


Clonamos el repositorio de github o bitbucket
```ssh
	git clone <url>
```
Clonamos el repositorio de github o bitbucket ?????
```ssh
	git clone <url> git-demo
```

## GIT ADD


Añadimos todos los archivos para el commit
```ssh
	git add .
```
Añadimos el archivo para el commit
```ssh
	git add <archivo>
```
Añadimos todos los archivos para el commit omitiendo los nuevos
```ssh
	git add --all 
```
Añadimos todos los archivos con la extensión especificada
```ssh
	git add *.txt
```
Añadimos todos los archivos dentro de un directorio y de una extensión especifica
```ssh
	git add docs/*.txt
```
Añadimos todos los archivos dentro de un directorios
```ssh
	git add docs/
```
## GIT COMMIT

Cargar en el HEAD los cambios realizados
```ssh
	git commit -m "Texto que identifique por que se hizo el commit"
```
Agregar y Cargar en el HEAD los cambios realizados
```ssh
	git commit -a -m "Texto que identifique por que se hizo el commit"
```
De haber conflictos los muestra
```ssh
	git commit -a 
```
Agregar al ultimo commit, este no se muestra como un nuevo commit en los logs. Se puede especificar un nuevo mensaje
```ssh
	git commit --amend -m "Texto que identifique por que se hizo el commit"
```
## GIT PUSH

Subimos al repositorio
```ssh
	git push <origien> <branch>
```
Subimos un tag
```ssh
	git push --tags
```
## GIT LOG

Muestra los logs de los commits
```ssh
	git log
```
Muestras los cambios en los commits
```ssh
	git log --oneline --stat
```
Muestra graficos de los commits
```ssh
	git log --oneline --graph
```
## GIT DIFF

Muestra los cambios realizados a un archivo
```ssh
	git diff
	git diff --staged
```
## GIT HEAD

Saca un archivo del commit
```ssh
	git reset HEAD <archivo>
```
Devuelve el ultimo commit que se hizo y pone los cambios en staging
```ssh
	git reset --soft HEAD^
```
Devuelve el ultimo commit y todos los cambios
```ssh
	git reset --hard HEAD^
```
Devuelve los 2 ultimo commit y todos los cambios
```ssh
	git reset --hard HEAD^^
```
Rollback merge/commit
```ssh
	git log
	git reset --hard <commit_sha>
```
## GIT REMOTE

Agregar repositorio remoto
```ssh
	git remote add origin <url>
```
Cambiar de remote
```ssh
	git remote set-url origin <url>
```
Remover repositorio
```ssh
	git remote rm <name/origin>
```
Muestra lista repositorios
```ssh
	git remote -v
```
Muestra los branches remotos
```ssh	
	git remote show origin
```
Limpiar todos los branches eliminados
```ssh
	git remote prune origin 
```
## GIT BRANCH

Crea un branch
```ssh
	git branch <nameBranch>
```
Lista los branches
```ssh
	git branch
```
Comando -d elimina el branch y lo une al master
```ssh
	git branch -d <nameBranch>
```
Elimina sin preguntar
```ssh
	git branch -D <nameBranch>
```
## GIT TAG

Muestra una lista de todos los tags
```ssh
	git tag
```
Crea un nuevo tags
```ssh
	git tag -a <verison> - m "esta es la versión x"
```
## GIT REBASE

Los rebase se usan cuando trabajamos con branches esto hace que los branches se pongan al día con el master sin afectar al mismo

Une el branch actual con el mastar, esto no se puede ver como un merge
```ssh
	git rebase
```
Cuando se produce un conflicto no das las siguientes opciones:

cuando resolvemos los conflictos --continue continua la secuencia del rebase donde se pauso
```ssh	
	git rebase --continue 
```
Omite el conflicto y sigue su camino
```ssh
	git rebase --skip
```
Devuelve todo al principio del rebase
```ssh
	git reabse --abort
```
Para hacer un rebase a un branch en especifico
```ssh	
	git rebase <nameBranch>
```
## OTROS COMANDOS

Lista un estado actual del repositorio con lista de archivos modificados o agregados
```ssh
	git status
```
Quita del HEAD un archivo y le pone el estado de no trabajado
```ssh
	git checkout -- <file>
```
Crea un branch en base a uno online
```ssh
	git checkout -b newlocalbranchname origin/branch-name
```
Busca los cambios nuevos y actualiza el repositorio
```ssh
	git pull origin <nameBranch>
```
Cambiar de branch
```ssh
	git checkout <nameBranch/tagname>
```
Une el branch actual con el especificado
```ssh
	git merge <nameBranch>
```
Verifica cambios en el repositorio online con el local
```ssh
	git fetch
```
Borrar un archivo del repositorio
```ssh
	git rm <archivo> 
```

## Fork

Descargar remote de un fork
```
	git remote add upstream <url>
```

Merge con master de un fork
```
	git fetch upstream
	git merge upstream/master
```
-->
