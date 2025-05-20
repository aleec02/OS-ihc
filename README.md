# OS-ihc
En algunas partes se hará referencia al google docs: [TB1 - google docs](https://docs.google.com/document/d/1cf9X94sHGeMCrpE4YsaDYfo1z6iy1iaMGh2xQO5GhUA/edit?usp=sharing) , no necesitas acceder a ello, pq todos los comandos a copiar y pegar están aquí, pero solo como referencia de los screenshots que debería salirte como resultado si estás en duda / porsiaca.

asumir que todos los comandos son obligatorios para seguir el sgte paso a menos que diga explicitamente opcional. los comandos opcionales son más que todo de corroboración / ver si algo está funcionando.

cada vez que menciono los mayores y menores no escribir los mayores y menores en sí xd, esto es solo una representacion del dato a reemplazar : `<dato a reemplazar>` para que se distinga.


## configuración básica de VM
- Primero ir a [Free Services Microsoft Azure](https://portal.azure.com/#view/Microsoft_Azure_Billing/FreeServicesBlade); si tienes 3 o más VMs ya creadas te aparecerá error al crear una nueva, si es así borrar la menos relevante.
    - Crear Grupo de Nuevos recursos y poner nombre
    - Tipo de autentificación: Contraseña
    - **Anota el nombre de usuario**; esto es importante para luego acceder a la VM en tu pc así `ssh -o PubkeyAuthentication=no <nombredeusuario>@IPpublicaDeVM` -> `pedirá contraseña`. Esto diré/volveré a explicar de nuevo más a detalle cuando toque.
    - Como el tiempo del examen puede que quede corto esta vez haremos la corroboración de acceso a contraseña , NO con archivo .pem pq mucho tiempo demora configurar.
    - El resto de campos dejalos tal como está (comos los puertos de entrada SSH, etc) y solo acepta el botón azul.
    - Una vez que se haya creado la VM solo anota la IP pública.
- Ahora ejecuta esto en tu Windows terminal o tmb conocido como `wt`, NO utilices CMD pq se te va a ser difícil copiar y pegar texto. Si aún no lo tienes instalado hazlo [Windows Terminal \| Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?hl=en-US&gl=US).
```powershell
ssh -o PubkeyAuthentication=no <nombredeusuario>@<IPpublicaDeVM>
```


## instalaciones
Una ves dentro del VM, instala lo siguiente, tu solo copia y pega los comandos y Enter, si te pide confirmación en cualquier instalacion de paquete con `y/n` en `sudo apt install <paquete(s)>` tu solo escribe `y` (de yes/sí) y Enter.

## básicas
- básico; actualizar paquetes del sistema operativo para evitar errores de desincronización
```zsh
sudo apt upgrade; sudo apt update 
```
- obtener editor de texto básico `nvim`; el cual utilizaremos principalmente para cambiar las entradas permitidas de IP. tmb sirve para codear en general pero esto será explicado cuanod sea relevante. 
```zsh
sudo apt install neovim
```


## comandos/instrucciones por capa
### herramientas específicas de Capas




#### comandos/instrucciones en la Capa de Datos
(solo instalar en la capa correspondiente)
- Herramientas especificas a la **Capa de Datos** with `sudo apt install`. Instalar en respectiva VM


para instalar mysql server en VM
```zsh
sudo apt install mysql-server
```

para iniciar el servicio de MySQL
```zsh
sudo systemctl start mysql
```

(opcional) para ver el estado de que mysql está funcionando
```zsh
sudo systemctl status mysql ## solo escribe `q` para salir
```



ahora puedes acceder al sql con 
```zsh
sudo mysql
```
el comando de arriba quedará invalido con las sgtes configuraciones que vamos a hacer. 
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<contraseña a utilizar>';
exit;
```
A partir de ahora el siguiente comando será válido y NO `sudo mysql`
```zsh
mysql -h localhost -u root -p
```
antes de copiar y pegar, de nuevo dentro del `mysql`; reemplaza <usuario> y <contraseña> aquí
```sql
CREATE USER '<usuario>'@'%' IDENTIFIED BY '<contraseña>';
GRANT ALL PRIVILEGES ON *.* TO '<usuario>'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
Luego se puede ingresar de la siguiente manera, **esta es la manera en la vas a ingresar desde ahora en adelante**, el resto queda invalido. estas credenciales aparte del mysql de linea de comandos tambien sirven para Workbench 
```zsh
mysql -h localhost -u <usuarioNombre> -p.
```
no usaremos workbench aqui y solo mysql en la línea de comandos por el tiempo, pero si pide Workbench entonces hacer referencia a la pg 47-48 del _google docs - TB1_.
    - el cual es basicamente agregar un nombre de conexión, hostname es tu IP pública, port 3306 (que es del sql), el username es el que pusiste antes del `FLUSH PRIVELEGES`, el valido. ppassword clickea store in vault y escibre tu contraseña sql. connection method TCP/IP.

con el ultimo comando mencionado deberías estar dentro del mysql (osea `mysql>`). allí copias y pegas el <archivo.sql> que tengas los contenidos, por mientras vamos a utilizar el siguiente .sql como ejemplo, si el profe te pasa un .sql bien; si tu tienes que crear el .sql F, haz eso primero
[ElBaul-Workbench.sql at main · aleec02/OS\_ElBaul\_PC2 · GitHub](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/SQL_data/ElBaul-Workbench.sql)

tu solo copia y pega los contenidos del sql cuando estes en `mysql>`. solo no te olvides todas las sentencias sql deben de terminar en un punto y coma `;` y hazle otro enter a la ultima declaracion sql.

Si quieres corroborar que todas las tablas estén entonces haz
```sql
SHOW TABLE STATUS;
```

por mientras salte de allí. para salir de `mysql>` recuerda escribe 
```sql
exit ## y enter claro
```

#### comandos/instrucciones en la Capa de Aplicacion
Instalar en respectiva VM para el stack LAMP
```zsh
sudo apt install php libapache2-mod-php php-mysql
```
todos estos servicios deberían de correr automáticamente a diferencia de mysql, (opcional) pero por si quieren verificar
```zsh
systemctl status apache2 ## solo escribe `q` para salir
```
lo bueno de estas paquetes es que a diferencia de algo como python no necesitas llamar las bibliotecas, todo es automatico una vez que instales lo de arriba, en tus archivos .php no vas a tener que importar nada


** ir a pagina 50 del google docs de la TB1

en cuanto al codigo que queda hasta la PC2 que es solo mostrar los datos de tablas [GitHub - aleec02/OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2) como pueden observar el codigo de cada tabla como `pago.php` por ejemplo es bastante repetitivo pq cada entidad solo muestra tablas. Cada entidad tiene su respectivo .php, y cada php que corresponda a una entidad en bastante repetitivo en su estructura, lo cual nos conviene en entender/memorizar syntaxis, etc. 


los archivos importantes/diferentes allí son:
    - [db_connection.php OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/db_connection.php)
    - [index.php OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/index.php); la cual es el menos "puro PHP" pq tiene html embedido
    - [config.php OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/config.php)
    - [functions.php OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/functions.php)


llamadas comunes
```
require_once 'includes/config.php';
require_once 'includes/db_connection.php';
require_once 'includes/functions.php';
```
todos los archivos que aparecen en la subcarpeta de PHP_web van en realidad en `var/www/html`, para moverse a esta carpeta
```zsh
cd var/www/html
```
en la estructura de este directorio todos los archivos .php van en el root de `html`, solo si el profe dice llegar a la capa de presentación es que haremo subdirectorios dentro de `html`; los cuales serán detallados luego.


#### comandos/instrucciones en la Capa de presentación
- la **capa de presentación** no existe en mi caso para poder ahorrar tiempo y configuración complicada con XAMPP u otra herramienta. lo que debería de ir en la capa de Presentación está llendo en la capa de aplicación.
    - esto quiere decir que las .css y el .js están llendo dentro de los scripts de .php, como referencia revisar los archivos de la repo de la capa ElBaul: [GitHub - aleec02/App-ElBaul](https://github.com/aleec02/App-ElBaul)
pero estoy poniendo en esta seccion solo para dividir mejor el README

si pide perfil de admin y user como el proyecto entonces tendrás que crear 4 subdirectorios dentro de html, los nombres son, luego se dará los comandos para crear estas carpetas
```
admin
css ## esta carpeta realmente no va a ser muy utilizada por falta de factorizacion en los .css pero fue
includes
user
```

```zsh
mkdir admin css includes user
```

