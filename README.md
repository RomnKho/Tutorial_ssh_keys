### Como configurar una clave ssh

Las claves ssh sirven para hacer push desde tu repositorio local y que se identifique automáticamente que eres tú sin necesidad de poner usuario y token cada vez.

Los pasos para configurarlo son los siguientes (solo para un repositorio):

Abre la terminal y ejecuta el siguiente comando. Ten en cuenta que debe ser con el correo vinculado a tu github.
```
ssh-keygen -t ed25519 -C "mi_correo@ejemplo.com"
```
Esto creará su clave ssh. Te pedirá donde guardarlo por lo que puedes decidir en donde hacerlo pero, por lo general, guarda tus claves en /home/nombre_usuario/.ssh/nombre_fichero. Te pedirá además configurar una passphrase, esto es una contraseña que puedes o no poner.

Después de haber creado la clave dirigete a la carpeta donde creaste la clave y comprueba que al archivo se ha creado correctamente.
```
cd /home/nombre_usuario/.ssh
```
Si está todo bien ahora dirigete a github/ajustes/claves ssh y gpg y dale a crear una nueva clave ssh. Ahí deberás pegar en el cuadrado grande lo que está dentro del archivo .pub que se creó antes. Se tiene que parecer a lo siguiente.
```
SHA256: contraseña... mi_correo@ejemplo.com
```
Ahora solo falta crear tu repositorio en github y clonarlo. Para esto, no copies el link https sino el ssh. Este deberá ser del tipo.
```
git@github.com:nombre_usuario/nombre_repo.git
```
Clona el repositorio con
```
git clone git@github.com:nombre_usuario/nombre_repo.git
```
y ya podrás hacer push sin la necesidad de ingresar token cada vez.

### Problemas comúnes

#### Repositorio ya creado

Si ya tienes el repositorio creado con https lo único que tienes que hacer es cambiar el destino del push y del fetch. Para ello, desde la terminal entra en tu repositorio clonado y ejecuta lo siguiente.
```
git remote -v
```
Esto generará una salida como esta.
```
origin  https://github.com/nombre_usuario/nombre_repo.git (fetch)
origin  https://github.com/nombre_usuario/nombre_repo.git (push)
```
Esto confirma que esté clonado con https. Ahora lo cambiaremos a ssh de la siguiente forma.
```
git remote set-url origin git@github.com:nombre_usuario/nombre_repo.git
```
Comprobamos de nuevo a que está asociado el fetch y el push con
```
git remote -v
```
Y deberá salir algo por el estilo.
```
origin  git@github.com:nombre_usuario/nombre_repo.git (fetch)
origin  git@github.com:nombre_usuario/nombre_repo.git (push)
```
Y listo ya deberías hacer push en tu repositorio local sin la necesidad de usar token. Aquí no hace falta que clones el repositorio de nuevo.

#### No me deja hacer git clone

Eso es porque seguramente no tengas configurada la identidad del agente. Comprueba si existe con
```
ssh-add -l
```
En el caso de que no (lo más probable) debes ejecutar 
```
eval "$(ssh-agent -s)
```
para comprobar el pid del agente (si existe). Así ahora simplemente asociaremos la identidad de este con el repositorio.
```
ssh-add /home/nombre_usuario/.ssh/nombre_fichero
```
Ahora si haces 
```
ssh-add -l
```
te debería salir que está todo correctamente configurado. Ahora ya puedes clonar tu repositorio y hacer push sin la necesidad de introducir el token cada vez.
