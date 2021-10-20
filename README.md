# Git 101 rsancho Edicion

https://www.freecodecamp.org/espanol/news/10-comandos-de-git-que-todo-desarrollador-deberia-saber/

Git es importante si programas a diario (especialmente si trabajas en equipo) y se usa extensamente en la industria de sw.

Dominar git requiere tiempo. Pero -solo- algunos comandos son los más frecuentes (algunos a diario varias veces). 

Podriamos (***para github***)  usar **`git`** con un cliente mas moderno, como **`gh`** pero vamos a usar el comando git clasico, dandole soporte con ssh: un buen recetario es [**este**](https://git-scm.com/docs/git-remote.html#_examples).

Siguen comandos de Git rutinarios -normales en devel workflow- -más usados- 

## 0: Conseguir este material (este README.md y (pero dentro de) su todo su repo) 

Se puede hacer de muchos modos, pero vamos a usar este:

+ hacemos login en github
+ Hacemos un fork de [este repo] (https://github.com/rsancho64/git.n.github.101) y ya tenemos nuestra copia del repo (Es *otro* repo: homonimo, pero de otro usuario)

### remoto a local

+ En local, sin ssh como prerequisito (solo git) ya podemos hacer un acceso al repo. Sea por ejemplo https://github.com/USUARIO/git.n.github.101. Entonces:

```bash
mkdir folder
cd folder
git init 
git remote set-url origin git@github.com:USUARIO/git.n.github.101
git fetch 
git checkout main
ls  # README.md en local
```

### edicion local

Cambiamos la linea 1 de **este fichero** para personalizarla un poco con nuestro nombre

-- # Git 101 rsancho Edicion
++ # Git 101 USUARIO edicion.

### local a remoto: 

## 1: ssh config:

[video](https://www.youtube.com/watch?v=G69dfwG2DJ4)

En la instalacion de debian 11 ya tengo **`openssh-client`** (no hace falta -server); y tengo comandos:

```bash
$ apropos ssh
rcp (1)               - OpenSSH secure file copy
rlogin (1)            - OpenSSH remote login client
rsh (1)               - OpenSSH remote login client
scp (1)               - OpenSSH secure file copy
sftp (1)              - OpenSSH secure file transfer
slogin (1)            - OpenSSH remote login client
ssh (1)               - OpenSSH remote login client
ssh-add (1)           - adds private key identities to the OpenSSH authenticat...
ssh-agent (1)         - OpenSSH authentication agent
ssh-argv0 (1)         - replaces the old ssh command-name as hostname handling
ssh-copy-id (1)       - use locally available keys to authorise logins on a re...
ssh-keygen (1)        - OpenSSH authentication key utility
ssh-keyscan (1)       - gather SSH public keys from servers
ssh-keysign (8)       - OpenSSH helper for host-based authentication
ssh-pkcs11-helper (8) - OpenSSH helper for PKCS#11 support
ssh-sk-helper (8)     - OpenSSH helper for FIDO authenticator support
ssh_config (5)        - OpenSSH client configuration file
XtIsShell (3)         - obtain and verify a widget's class
```
receta: [connecting-to-github-with-ssh](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/about-ssh)

receta: [generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent):

### generating new ssh-k:

```bash
ssh-keygen -t ed25519 -C "rsancho@iesrioarba.es"
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (~/.ssh/id_ed25519): 
# Enter passphrase (empty for no passphrase): *****************
# Enter same passphrase again: *****************
# Your identification has been saved in ~/.ssh/id_ed25519
# Your public key has been saved in ~/.ssh/id_ed25519.pub
# The key fingerprint is:
# SHA256:5CPr7vDfdBd+6O1sVWsUFWOjaECPuUQxav2RxGBw9kA rsancho@iesrioarba.es
```

```bash
ls -l ~/.ssh
# total 12K
# -rw------- 1 ray ray 464 sep 13 17:30 id_ed25519
# -rw-r--r-- 1 ray ray 103 sep 13 17:30 id_ed25519.pub
# -rw-r--r-- 1 ray ray 884 sep 13 18:38 known_hosts
```

### adding ssh-k to ssh-agent:

de [aqui](https://kbroman.org/github_tutorial/pages/first_time.html)

```bash
eval "$(ssh-agent -s)" # start ssh-agent in background
# Agent pid 26914
ssh-add ~/.ssh/id_ed25519
# Enter passphrase for ~/.ssh/id_ed25519: *****************
# Identity added: ~/.ssh/id_ed25519 (USUARIO@iesrioarba.es)
```
### setting id to [local] git cli
```bash
git config --global user.name "name-in-github"   # "USUARIO"
git config --global user.email "email-in-github" # "USUARIO@iesrioarba.es"
git config --global color.ui true
```
### give our public key to github

```bash
sudo apt install wl-clipboard     # the WayLand copypaster 4 deb11
wl-copy < ~/.ssh//id_ed25519.pub  # copy to clipboard
```
now ++ & label ssh public key -onlogin- at account settings https://github.com/settings/keys](https://github.com/settings/keys)

```bash        
    $ ssh -T git@github.com
    Your password: *****************
    Hi rsancho64! You've successfully authenticated, but GitHub does not provide shell access.
```
### rutina tras la conexion

de [aqui] https://kbroman.org/github_tutorial/pages/routine.html

## 1. `**clone**` 

clone descarga el código fuente existente desde un repositorio remoto (como Github, por ejemplo). Realiza una copia idéntica de la última
versión de un proyecto en un repositorio, y la guarda en tu ordenador.

Hay un par de formas de descargar el código fuente, pero clonar con https es cómodo: desde Github, clic en **botón verde** (clonar o descargar), copiar la URL de la caja y pegarla en CLI:

```bash
git clone <https://url-repo.git>
```
Esto hará una copia del proyecto en local y se podrá trabajar ahi.

## 1. `**branch**` 

Las ramas (branch) son esenciales en git. Usandolas varios desarrolladores pueden trabajar en paralelo en el mismo proyecto (simultáneamente). [Una buena explicacion](https://www.nobledesktop.com/learn/git/git-branches).

NOTA: un **branch** no es lo mismo que un **fork**:

+ Un **branch** -*rama* o *ruta*(de un repo))- es un objeto ligero, que puede ser temporal y puede terminar desapareciendo, o no. 
*Fisicamente es una anotación solo de los camibios relativos a su rama padre.*

+ Un **fork** (*clon separado*) es una copia completa (profunda) de un repo (tomada en un momento dado, lo es de su ultima versión).
*Fisicamente es una copia completa de su repo padre.* A partir de su creación, evolucionan independientemente.

```bash
cd <folder> # en la raiz del repo, donde hay una subcarpeta .git
git branch <nombre-de-la-rama>  # nueva rama (solo en local)
```
Ejemplo: creamos la rama `fooBranch` (en local)

```bash
$ git branch fooBranch
$ git branch 
  fooBranch
* main
```
Para enviar la nueva rama al repo remoto, hará falta un **push**, pero antes usamos branch sin argumentos (listar ramas) y **status**

```bash
$ git status
En la rama main
Tu rama está actualizada con 'origin/main'.

Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
    modificado:     README.md

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
    git101.md

sin cambios agregados al commit (usa "git add" y/o "git commit -a")
$ git add README.md 
$ git status
En la rama main
Tu rama está actualizada con 'origin/main'.

Cambios a ser confirmados:
  (usa "git restore --staged <archivo>..." para sacar del área de stage)
    modificado:     README.md

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
    git101.md

$ git add git101.md 
```bash

```bash
git push <nombre-remoto> <nombre-rama>
```
**OOOPS** desde August 13, deprecated passw authentication...

```bash
    $ git push
    Username for 'https://github.com': rsancho64
    Password for 'https://rsancho64@github.com': 
    remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
    remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
    fatal: Autenticación falló para 'https://github.com/rsancho64/rsancho64.git/'
```
,,: hace falta [operar en https](https://techglimpse.com/git-push-github-token-based-passwordless/):

`git push https://<GITHUB_ACCESS_TOKEN>@github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git`

```bash
    $ git push https://ghp_************************************@github.com/rsancho64/rsancho64.git
    Enumerando objetos: 5, listo.
    Contando objetos: 100% (5/5), listo.
    Compresión delta usando hasta 4 hilos
    Comprimiendo objetos: 100% (2/2), listo.
    Escribiendo objetos: 100% (3/3), 549 bytes | 549.00 KiB/s, listo.
    Total 3 (delta 0), reusado 0 (delta 0), pack-reusado 0
    To https://github.com/rsancho64/rsancho64.git
        39342cd..3392dcd  main -> main
```

**Visualización de ramas**:

    git branch
    git branch --list

**Borrar una rama**:

    git branch -d <nombre-de-la-rama>

## ****3. Git checkout**** {#3-git-checkout}

Este es también uno de los comandos más utilizados en Git. Para trabajar
en una rama, primero tienes que cambiarte a ella. Usaremos **git
checkout** principalmente para cambiarte de una rama a otra. También lo
podemos usar para chequear archivos y commits.

    git checkout <nombre-de-la-rama>

Hay algunos pasos que debes seguir para cambiarte exitosamente entre
ramas:

-   Los cambios en tu rama actual tienen que ser confirmados o
    almacenados en el guardado rápido (stash) antes de que cambies de
    rama.
-   La rama a la que te quieras cambiar debe existir en local.

**Hay también un comando de acceso directo que te permite crear y
cambiarte a esa rama al mismo tiempo:**

    git checkout -b <nombre-de-tu-rama>

Este comando crea una nueva rama en local (-b viene de rama (branch)) y
te cambia a la rama que acabas de crear.

## ****4. Git status**** {#4-git-status}

El comando de git status nos da toda la información necesaria sobre la
rama actual.

    git status

Podemos encontrar información como:

-   Si la rama actual está actualizada
-   Si hay algo para confirmar, enviar o recibir (pull).
-   Si hay archivos en preparación (staged), sin preparación(unstaged) o
    que no están recibiendo seguimiento (untracked)
-   Si hay archivos creados, modificados o eliminados

![git status nos da información acerca del archivo y las
ramas](https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-status-1.png){.kg-image
srcset="https://www.freecodecamp.org/espanol/news/content/images/size/w600/2020/12/git-status-1.png 600w, https://www.freecodecamp.org/espanol/news/content/images/size/w1000/2020/12/git-status-1.png 1000w, https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-status-1.png 1476w"
sizes="(min-width: 720px) 720px"}

## ****5. Git add**** {#5-git-add}

Cuando creamos, modificamos o eliminamos un archivo, estos cambios
suceden en local y no se incluirán en el siguiente commit (a menos que
cambiemos la configuración).

Necesitamos usar el comando git add para incluir los cambios del o de
los archivos en tu siguiente commit.

**Añadir un único archivo:**

    git add <archivo>

**Añadir todo de una vez:**

    git add -A

Si revisas la captura de pantalla que he dejado en la sección 4, verás
que hay nombres de archivos en rojo - esto significa que los archivos
sin preparación. Estos archivos no serán incluidos en tus commits hasta
que no los añadas.

**Para añadirlos, necesitas usar el git add:**

![Los archivos en verde han sido añadidos a la preparación gracias al
git
add](https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-add.png){.kg-image
srcset="https://www.freecodecamp.org/espanol/news/content/images/size/w600/2020/12/git-add.png 600w, https://www.freecodecamp.org/espanol/news/content/images/size/w1000/2020/12/git-add.png 1000w, https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-add.png 1480w"
sizes="(min-width: 720px) 720px"}

****Important**e**:** El comando git add no cambia el repositorio y los
cambios que no han sido guardados hasta que no utilicemos el comando de
confirmación git commit.**

## ****6. Git commit**** {#6-git-commit}

Este sea quizás el comando más utilizado de Git. Una vez que se llega a
cierto punto en el desarrollo, queremos guardar nuestros cambios (quizás
después de una tarea o asunto específico).  

Git commit es como establecer un punto de control en el proceso de
desarrollo al cual puedes volver más tarde si es necesario.

También necesitamos escribir un mensaje corto para explicar qué hemos
desarrollado o modificado en el código fuente.

    git commit -m "mensaje de confirmación"

****Important**e**: Git commit** guarda tus cambios únicamente en
local.**

## ****7. Git push**** {#7-git-push}

Después de haber confirmado tus cambios, el siguiente paso que quieres
dar es enviar tus cambios al servidor remoto. Git push envía tus commits
al repositorio remoto.

    git push <nombre-remoto> <nombre-de-tu-rama>

De todas formas, si tu rama ha sido creada recientemente, puede que
tengas que cargar y subir tu rama con el siguiente comando:

    git push --set-upstream <nombre-remoto> <nombre-de-tu-rama>

or

    git push -u origin <nombre-de-tu-rama>

****Important**e**: Git push** solamente carga los cambios que han sido
confirmados.**

## ****8. Git pull**** {#8-git-pull}

El comando ****git pull**** se utiliza para recibir actualizaciones del
repositorio remoto. Este comando es una combinación del ****git
fetch**** y del ****git merge**** lo cual significa que cundo usemos el
git pull recogeremos actualizaciones del repositorio remoto (git fetch)
e inmediatamente aplicamos estos últimos cambios en local (git merge).

    git pull <nombre-remoto>

**Esta operación puede generar conflictos que tengamos que resolver
manualmente.**

## ****9. Git revert**** {#9-git-revert}

A veces, necesitaremos deshacer los cambios que hemos hecho. Hay varias
maneras para deshacer nuestros cambios en local y/o en remoto
(dependiendo de lo que necesitemos), pero necesitaremos utilizar
cuidadosamente estos comandos para evitar borrados no deseados.

Una manera segura para deshacer nuestras commits es utilizar **git
revert**. Para ver nuestro historial de commits, primero necesitamos
utilizar el  ****git log \-- oneline:****

![histórico de git en mi rama
master](https://www.freecodecamp.org/espanol/news/content/images/2020/12/histo-rico-git.png){.kg-image
srcset="https://www.freecodecamp.org/espanol/news/content/images/size/w600/2020/12/histo-rico-git.png 600w, https://www.freecodecamp.org/espanol/news/content/images/size/w1000/2020/12/histo-rico-git.png 1000w, https://www.freecodecamp.org/espanol/news/content/images/2020/12/histo-rico-git.png 1484w"
sizes="(min-width: 720px) 720px"}

Entonces, solo necesitamos especificar el código de comprobación que
encontrarás junto al commit que queremos deshacer:

    git revert 3321844

Después de esto, verás una pantalla como la de abajo -tan solo presiona
****shift + q**** para salir:

![](https://www.freecodecamp.org/news/content/images/2020/01/resim-2.png){.kg-image}

El comando git revert deshará el commit que le hemos indicado, pero
creará un nuevo commit deshaciendo la anterior:

![commit generado con el git
revert](https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-revert.png){.kg-image
srcset="https://www.freecodecamp.org/espanol/news/content/images/size/w600/2020/12/git-revert.png 600w, https://www.freecodecamp.org/espanol/news/content/images/size/w1000/2020/12/git-revert.png 1000w, https://www.freecodecamp.org/espanol/news/content/images/2020/12/git-revert.png 1484w"
sizes="(min-width: 720px) 720px"}

La ventaja de utilizar ****git revert**** es que no afecta al commit
histórico. Esto significa que puedes seguir viendo todos los commits en
tu histórico, incluso los revertidos.

Otra medida de seguridad es que todo sucede en local a no ser que los
enviemos al repositorio remoto. Por esto es que git revert es más seguro
de usar y es la manera preferida para deshacer los commits.

## ****10. Git merge**** {#10-git-merge}

Cuando ya hayas completado el desarrollo de tu proyecto en tu rama y
todo funcione correctamente, el último paso es fusionar la rama con su
rama padre (dev o master). Esto se hace con el comando `git merge`.

Git merge básicamente integra las características de tu rama con todos
los commits realizados a las ramas dev (o master).  Es importante que
recuerdes que tienes que estar en esa rama específica que quieres
fusionar  con tu rama de características.

Por ejemplo, cuando quieres fusionar tu rama de características en la
rama dev:

**Primero, debes cambiarte a la rama dev:**

    git checkout dev

**Antes de fusionar, debes actualizar tu rama dev local:**

    git fetch

**Por último, puedes fusionar tu rama de características en la rama
dev:**

    git merge <nombre-de-la-rama>

**Pista**:** Asegúrate de que tu rama dev tiene la última versión antes
de fusionar otras ramas, si no, te enfrentarás a conflictos u otros
problemas no deseados.**

Aquí están mis 10 comandos de git más usados cuando me enfrento a la
programación en mi día a día. Hay muchas más cosas que aprender sobre
Git y las explicaré más adelante en oros artículos.

**Si quieres aprender más sobre el desarrollo web, ¡[puedes seguirme en
Youtube](https://www.youtube.com/channel/UC1EgYPCvKCXFn8HlpoJwY3Q)!**

¡Gracias por leerme!

Traducido del artículo de [**Cem
Eygi**](https://www.freecodecamp.org/news/author/cemeygi/) **- [10 Git
Commands Every Developer Should
Know](https://www.freecodecamp.org/news/10-important-git-commands-that-every-developer-should-know/)**


## repo brand new from scratch

```bash
$ cd /git.n.github.101
$ git init # produce este warning:
ayuda: Using 'master' as the name for the initial branch. This default branch name
ayuda: is subject to change. To configure the initial branch name to use in all
ayuda: of your new repositories, which will suppress this warning, call:
ayuda: 
ayuda:  git config --global init.defaultBranch <name>
ayuda: 
ayuda: Names commonly chosen instead of 'master' are 'main', 'trunk' and
ayuda: 'development'. The just-created branch can be renamed via this command:
ayuda: 
ayuda:  git branch -m <name>
Inicializado repositorio Git vacío en ~/git.n.github.101/.git/

$ rm -Rf .git # empezamos de nuevo
$ git config --global init.defaultBranch main
$ git init
Inicializado repositorio Git vacío en ~/git.n.github.101/.git/
```
[nota](https://stackoverflow.com/questions/7152607/git-force-push-current-working-directory)

Creamos el repo vacio en github (rsancho64/git.n.github.101 public)

```bash
$ git config receive.denyCurrentBranch ignore
$ ls
-rw-r--r-- 1 ray ray 20K sep 13 23:36 README.md
$ git status
En la rama main
No hay commits todavía
Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
  <p style="color:red">README.md</p>
no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
$ git add README.md 
$ git commit
[main (commit-raíz) 5996d07] git n github 101 tutorial
 1 file changed, 521 insertions(+)
 create mode 100644 README.md
$ git branch -M main
$ git status
...
  <p style="color:green">README.md</p>
...
$ git remote add origin https://github.com/rsancho64/git.n.github.101.git 
$ git push origin main
Username for 'https://github.com': rsancho64
Password for 'https://rsancho64@github.com': 
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: Autenticación falló para 'https://github.com/rsancho64/git.n.github.101.git/'
$  git push https://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@github.com/rsancho64/git.n.github.101.git 
Enumerando objetos: 3, listo.
Contando objetos: 100% (3/3), listo.
Compresión delta usando hasta 4 hilos
Comprimiendo objetos: 100% (2/2), listo.
Escribiendo objetos: 100% (3/3), 7.48 KiB | 1.87 MiB/s, listo.
Total 3 (delta 0), reusado 0 (delta 0), pack-reusado 0
To https://github.com/rsancho64/git.n.github.101.git
 * [new branch]      main -> main
```
