
[index](../README.md)

# Comandos Git

``` bash
git init #(creas un nuevo repositorio)

git config --global user.name “Nombre” #(configurar nombre del usuario)

git config --global user.email “Email” #(configurar nombre del email)

git config --list #(ver configuración de git)

git status #(estado del proyecto)

git add #(nombre del archivo con su extensión) (agrega el archivo nombrado al stage)

git add . #(agregamos todos los archivos al stage)

git commit -m “mensaje para enviar el proyecto” #(aquí queda guardado el stage del proyecto como se envio, permitirá restablecerlo desde esta versión)

git rm --cached (archivo con extensión) #(devuelve el archivo a como estaba antes de mandarlo)
```

.gitignore sirve para ignorar archivos y no se suban

``` bash
git log #(muestra la información del repositorio creado, muestra los cambios, quien los hizo y cuando)

git log --stat #(muestra las estadísticas del commit, líneas insertadas y borradas) (historial de los commit)

git log --oneline (muestra los códigos (ID) para volver tiempo atrás y muestra los commits simplificados)

git branch (nombre de la rama) #seleccionamos la rama madre donde queremos crear la otra rama

git branch #(ver cuantas ramas tenemos)

git checkout (nombre de la rama) #(cambia a la rama que quiero)(cambia entre versiones)

git checkout (ingresar código de la línea de tiempo) #(pero los commit desaparecen al usar esta opción que se usa para establecer el repositorio) **mucha precaución**

git checkout -b (nombre de la rama que queremos crear) #(realiza una copia del código) clear (limpia pantalla)

git merge (colocamos el nombre de la rama que queremos unir a la current branch) #(sirve para unir las ramas, se debe hacer desde la rama master para agregar las otras)

git branch -d (nombre de la rama que se quiere eliminar)

git rm -r --cached ./nombrecarpeta/ #(excluye la carpeta del master)

git restore (nombre del archivo ) #(deshace los cambios realizados y lo restaura a su estado anterior)

git git update-index --assume-unchanged ( nombre del archivo) #(Ignora los cambios de un archivo especifico)

git update-index --no-assume-unchanged ( nombre_archivo) #(Deja de ignorar los cambios de un archivo y los comienza a rastrear)


```

Nota: Después de usar gitignore

## Git Remote

- git remote set-url origin https://nueva-url.com (configurar la url del repositorio)

### Remote SSH

- git remote add origin (SSH)

- git add .

- git commit -m ‘mensaje’

- git push -u origin master (crear la rama master)

- ingresar clave

- archivo subido

### Remote HTTPS

- git remote add origin (HTTPS del repositorio **Extención ".git"**)

- git add .

- git commit -m ‘mensaje’

- git push -u origin master (crear la rama master)

## Descargar un repositorio

- Click en clone or download

- Copiar la dirección web

- Abrir git bash here (en la carpeta donde se va a clonar el proyecto)

- Comando git clone (dirección del proyecto a clonar)

## Comandos adicionales

git show (muestra los cambios entre los commit)
(cuando se unde enter y no ingresas ningún mensaje para el commit se realiza lo siguiente) Tecla ESC y Tecla I (i de insertar), esto permitirá agregar el mensaje.

git reset (se copia el código o ID que se ubica con el comando git log --oneline y se agrega), luego de insertar el código se coloca el tipo de reset, existen dos tipos de reset **–soft** y **–hard** (borra el commit seleccionado permanentemente)

git reset nombre_archivo (Devuelve el archivo de staged a unstaged)

git commit -am “mensaje” (funciona solo con los archivos que ya se les ha hecho add (para no tener que escribir los comandos por separado))

git reset --soft HEAD~1 (devolver el último commit manteniendo los cambios)

git reset --hard HEAD~1 (devolver el último commit eliminando los cambios)

git revert commit_id (revertir el commit ya subido)

git commit --amend -m “mensaje correcto” (cambiar el mensaje del último commit, también sirve para subir más cambios al mismo commit, sin necesidad de crear uno nuevo)

git pull (actualiza el repositorio con los cambios de los participantes)

git pull origin nombre_rama (actualiza el repositorio local con los cambios de una rama especifica del repositorio)

git push origin (nombre actual):(nombre de la rama en el respositorio) Subir una rama especifica en el repositorio

[fundamentos de Git](https://bluuweb.github.io/tutorial-github/01-fundamentos/#enlaces)

git config --global core.editor “code --wait “ (configurar vsc como editor de texto predeterminado)

git config --global -e (visualizar la configuración global en vsc)

git config --global core.autocrlf true (eliminar el carácter especial CR (salto de línea))

git config -h (muestra todas las posibles configuraciones)

git stash; se usa para almacenar cambios temporalmente para no generar un commit antes del merge con la rama mas actualizada del proyecto

```bash
# Guarda los cambios temporalmente como un stash
git stash

# Aplica los cambios del stash moviendolos al staged
git stash apply
```

## Comandos del Bash

```bash
clear #(limpia pantalla)

ls #(listar archivos y carpetas del directorio(carpeta))

rm #nombre_del_archivo (eliminar archivo de la fase staged (con commit))
```
