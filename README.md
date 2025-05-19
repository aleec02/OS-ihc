# OS-ihc

## configuración básica de VM
- Primero ir a [Free Services Microsoft Azure](https://portal.azure.com/#view/Microsoft_Azure_Billing/FreeServicesBlade); si tienes 3 o más VMs ya creadas te aparecerá error al crear una nueva, si es así borrar la menos relevante.
    - Si te sigue apareciendo error después de tener menos de 3 VMs tendrás que borrar la OS de una máquina existente, cambiarle el nombre y reattach HEAD of resources. 
    - [ ] _Si esto ocurre tendré que escribir como troubleshootear esto ;-;_  
- Anotar IP pública de la VM, esto es importante


## instalaciones
- básico
```zsh
sudo apt upgrade; sudo apt update 
```

- obtener zsh y editor de texto básico (si no alcanzas a hacer repo de github para poder codear en vscode, recomiendo que lo hagas en vscode con github en vez de nvim)
```zsh
sudo apt install zsh neovim fzf fd-find
sudo ln --symbolic $(which fdfind) /usr/local/bin/fd ## just apply the alias to get it as simple `fd` command
```

- install basic tools with `curl -sS <url> | zsh` to make this all work. tools mentioned: a. starship, b. zoxide.
```zsh
curl -sS https://starship.rs/install.sh | zsh
curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | zsh
```
- install optional with `sudo apt install`. tools mentioned: a. rg, b. tre.
```zsh
sudo apt install ripgrep tre
```

- install optional tools with `curl -sS <url> | zsh`. tools mentioned: a. lsd
```sh
curl -sS https://webi.sh/lsd | sh
source ~/.config/envman/PATH.env ## then just apply this command just as the message says
```

### OS tools
- Herramientas mencionadas en diagnosis work-related needs. Aplicable a todas la capas, pero más útil en la de Datos: 
```zsh
sudo apt install net-tools 
```

- Herramientas especificas a la **Capa de Datos** with `sudo apt install`. Instalar en respectiva VM
```zsh
sudo apt install mysql-server
```
** Ver pagina 42 hasta la 49 para ve operaciones de mySQL y la contraseña del google docs del TB1, es importante resaltar que
```zsh
- mysql -h localhost -u root -p ## te generará problemas cuando llegues al Workbench
- mysql -h localhost -u <usuarioNombre> -p ## funcionará normal para la inserción de datos con el SQL del Workbench
```

los comandos que quieres para iniciar el servicio de mysql, el status, stop son los siguientes:
```zsh
systemctl start mysql
systemctl status mysql
systemctl stop mysql
mysql -h localhost -u root -p
```
para poder entrar a los comandos de mysql


- Herramientas especificas a la **Capa de Aplicacion**.Instalar en respectiva VM para el stack LAMP
```zsh
sudo apt install php libapache2-mod-php php-mysql
```
todos estos servicios deberían de correr automáticamente a diferencia de mysql, pero por si quieren verificar

```zsh
systemctl status apache2
```
** ir a pagina 50 del google docs de la TB1

en cuanto al codigo que queda hasta la PC2 que es solo mostrar los datos de tablas [GitHub - aleec02/OS\_ElBaul\_PC2](https://github.com/aleec02/OS_ElBaul_PC2) como pueden observar el codigo de cada tabla como ``pago.php` por ejemplo es bastante repetitivo pq cada entidad solo muestra tablas. los archivos importantes/diferentes allí son:
    -  [OS\_ElBaul\_PC2/PHP\_web/db\_connection.php at main · aleec02/OS\_ElBaul\_PC2 · GitHub](https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/db_connection.php)
    - https://github.com/aleec02/OS_ElBaul_PC2/blob/main/PHP_web/index.php
    - estos:
        - require_once 'includes/config.php';
        - require_once 'includes/db_connection.php';
        - require_once 'includes/functions.php';



- la **capa de presentación** no existe en mi caso para poder ahorrar tiempo y configuración complicada con XAMPP u otra herramienta. lo que debería de ir en la capa de Presentación está llendo en la capa de aplicación.
    - esto quiere decir que las .css y el .js están llendo dentro de los scripts de .php, como referencia revisar los archivos de la repo de la capa ElBaul: [GitHub - aleec02/App-ElBaul](https://github.com/aleec02/App-ElBaul)





## .zshrc y profiles básicos

```zsh
## .zshrc
curl -s https://gist.githubusercontent.com/aleec02/11d86bd0d737c364c02df24733932d47/raw/3cc79d63b71d2ac554f91e6297adfd5b06194e6f/.zshrc%2520of%2520VM > $HOME/.zshrc

## zsh plugins
curl -s https://gist.githubusercontent.com/aleec02/c64511d001b0196f7d1a7388db8da62b/raw/ce438aefe541290b81c456792b80c7dd875effeb/zsh-plugins | zsh

## searchHistorial
curl -s https://gist.githubusercontent.com/aleec02/3c7d7f7f75bb4973b1e180a0a713a3f4/raw/0a3ab306139ccab26979de75afc8b6b5bf56dcd2/searchHistorial.sh
source $HOME/.zshrc
```


