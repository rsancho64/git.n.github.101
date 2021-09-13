https://www.freecodecamp.org/espanol/news/10-comandos-de-git-que-todo-desarrollador-deberia-saber/


# Git 101

Git es importante si programas a diario (especialmente si trabajas en equipo) y se usa extensamente en la industria de sw.
Dominar git requiere tiempo. Pero algunos comandos se utilizan más frecuentemente (algunos a diario varias veces). 
Podemos usar el comando git en la CLI dandole soporte ssh, pero el cliente gh es mas moderno ***para github*** 

> git is used for git in general you can use Bitbucket or GitLab or any provider with it you just add remote and you can push. But Github CLI gh is for Github; you can manage many features of Github from CLI e.g issues.
I personally prefer git as I am more comfortable and in many offices I don't use Github. https://git-scm.com/docs/git-remote.html#_examples

Siguen comandos de Git rutinarios -normales en devel workflow- -más usados- 

## pre0: crear cuenta en github.

## pre1: ssh config:

[video](https://www.youtube.com/watch?v=G69dfwG2DJ4)

Tengo openssh-client (no hace falta -server); tengo comandos:
```bash
$ apropos ssh
rcp (1)              - OpenSSH secure file copy
rlogin (1)           - OpenSSH remote login client
rsh (1)              - OpenSSH remote login client
scp (1)              - OpenSSH secure file copy
sftp (1)             - OpenSSH secure file transfer
slogin (1)           - OpenSSH remote login client
ssh (1)              - OpenSSH remote login client
ssh-add (1)          - adds private key identities to the OpenSSH authenticat...
ssh-agent (1)        - OpenSSH authentication agent
ssh-argv0 (1)        - replaces the old ssh command-name as hostname handling
ssh-copy-id (1)      - use locally available keys to authorise logins on a re...
ssh-keygen (1)       - OpenSSH authentication key utility
ssh-keyscan (1)      - gather SSH public keys from servers
ssh-keysign (8)      - OpenSSH helper for host-based authentication
ssh-pkcs11-helper (8) - OpenSSH helper for PKCS#11 support
ssh-sk-helper (8)    - OpenSSH helper for FIDO authenticator support
ssh_config (5)       - OpenSSH client configuration file
XtIsShell (3)        - obtain and verify a widget's class
```
[connecting-to-github-with-ssh](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/about-ssh)

[generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent):

### generating new ssh-k:
```bash
    $ ssh-keygen -t ed25519 -C "rsancho@iesrioarba.es"
    Generating public/private ed25519 key pair.
    Enter file in which to save the key (/home/ray/.ssh/id_ed25519): 
    Enter passphrase (empty for no passphrase): *****************
    Enter same passphrase again: *****************
    Your identification has been saved in /home/ray/.ssh/id_ed25519
    Your public key has been saved in /home/ray/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:5CPr7vDfdBd+6O1sVWsUFWOjaECPuUQxav2RxGBw9kA rsancho@iesrioarba.es
    $ ls -l ~/.ssh
    total 12K
    -rw------- 1 ray ray 464 sep 13 17:30 id_ed25519
    -rw-r--r-- 1 ray ray 103 sep 13 17:30 id_ed25519.pub
    -rw-r--r-- 1 ray ray 884 sep 13 18:38 known_hosts

```
### adding ssh-k to ssh-agent:

de [aqui](https://kbroman.org/github_tutorial/pages/first_time.html)

```bash
    $ eval "$(ssh-agent -s)" # start ssh-agent in background
    Agent pid 26914
    $ ssh-add ~/.ssh/id_ed25519
    Enter passphrase for /home/ray/.ssh/id_ed25519: *****************
    Identity added: /home/ray/.ssh/id_ed25519 (rsancho@iesrioarba.es)
```
### setting id to [local] git cli
```bash
    $ git config --global user.name "name-in-github"   # "rsancho64"
    $ git config --global user.email "email-in-github" # "rsancho@iesrioarba.es"
    $ git config --global color.ui true
```
### give our public key to github

```bash
    $ sudo apt install wl-clipboard     # Wayland copypaster @deb11
    $ wl-copy < ~/.ssh//id_ed25519.pub  # copy to clipboard
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
Para enviar la nueva rama al repositorio remoto, hará falta un **push**, pero antes usamos branch sin argumentos (listar ramas) y **status**

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

****Vi**sualización de ramas:**

    git branch
    
    git branch --list

**Borrar una rama**:****

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
:::

------------------------------------------------------------------------

::: post-full-author-header
::: {.section .author-card}
![Nora Gonzalo
Ciordia](/espanol/news/content/images/size/w100/2020/12/NGC.jpg){.author-profile-image}

::: {.section .author-card-content}
[[Nora Gonzalo Ciordia](/espanol/news/author/nora/)]{.author-card-name}

FullStack Dev Jr living in Madrid LinkedIn:
https://www.linkedin.com/in/noragonzalo/
:::
:::
:::

------------------------------------------------------------------------

Si este artículo te fue útil, [tweet it.]{#tweet-btn .cta-button}

::: learn-cta-row
Aprende a codificar de forma gratuita. El plan de estudios de código
abierto de freeCodeCamp ha ayudado a más de 40,000 personas a obtener
trabajos como desarrolladores. [Empieza]{#learn-to-code-cta .cta-button}
:::
:::
:::
:::

::: footer-container
::: footer-top
::: footer-desc-col
freeCodeCamp es una organización sin fines de lucro 501(c)(3) exenta de
impuestos apoyada por donantes (Número de identificación fiscal federal
de los Estados Unidos: 82-0779546)

Nuestra misión: ayudar a las personas a aprender a programar de forma
gratuita. Logramos esto creando miles de videos, artículos y lecciones
de programación interactivas, todas disponibles gratuitamente para el
público. También tenemos miles de grupos de estudio freeCodeCamp en todo
el mundo.

Las donaciones a freeCodeCamp van hacia nuestras iniciativas educativas
y ayudan a pagar servidores, servicios y personal.

Puedes hacer [una donación deducible de impuestos aquí]{#footer-donation
.inline}.
:::

::: trending-guides
::: col-header
Guías de Tendencias
:::

::: trending-guides-row
::: {.footer-col .footer-col-1}
[]{#article0} []{#article1} []{#article2} []{#article3} []{#article4}
[]{#article5} []{#article6} []{#article7} []{#article8} []{#article9}
:::

::: {.footer-col .footer-col-2}
[]{#article10} []{#article11} []{#article12} []{#article13}
[]{#article14} []{#article15} []{#article16} []{#article17}
[]{#article18} []{#article19}
:::

::: {.footer-col .footer-col-3}
::: footer-left
[]{#article20} []{#article21} []{#article22} []{#article23}
[]{#article24}
:::

::: footer-right
[]{#article25} []{#article26} []{#article27} []{#article28}
[]{#article29}
:::
:::
:::
:::
:::

::: footer-bottom
::: col-header
Nuestra Organización No Lucatriva
:::

::: footer-divider
:::

::: our-nonprofit
[Acerca]{#about} [Red de Ex-Alumnos]{#alumni} [Código
Abierto]{#open-source} [Tienda]{#shop} [Apoyo]{#support}
[Patrocinadores]{#sponsors} [Honestidad Académica]{#honesty} [Código de
Conducta]{#coc} [Política de Privacidad]{#privacy} [Términos de
Servicio]{#tos} [Política de Derechos de Autor]{#copyright}
:::
:::
:::
:::
