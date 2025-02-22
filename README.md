# Primer_Proyecto_Docker
Proyecto con contenedor e imágenes Docker

## Procedimiento

1.- Se empieza abriendo la terminal, en este caso el Símbolo del Sistema(CMD). Una vez dentro se descarga la imagen oficial de Ubuntu con este comando:
`docker pull ubuntu`

2.- Una vez descargada la imagen se procede a crear un contenedor en Ubuntu con:
`docker run -it --name ubuntu-container ubuntu`
Nota: `ubuntu-container` es el nombre que le esta asignando al contenedor

3.- Procedemos a iniciar el contendor ya que inicialmente se encuentra detenido, esto con el comando de:
`docker start ubuntu-container`
Nota: Se pueden visualizar los contenedores inicializados con `docker ps`

4.- Cuando ya está inicializado podemos ejecutar comandos dentro del contenedor gracias a:
`docker exec -it ubuntu-container bash`

5.- Una vez dentro del contenedor es necesario actualizar y configurar las herramientas necesarias para nuestro contenedor, para este caso se ocuparon principalmente:
```bash
apt update
apt upgrade
apt install nano
apt install curl
apt install wget
apt install git
```

6.- Una vez dentro del contenedor se procede a crear el archivo de “helloworld” con extensión de Python, para ello se instala Python con: `apt install python3` y verificamos la correcta instalación con `python3 --version`

7.- Creamos y editamos el archivo de helloworld.py, ambas cosas se logran con el comando de: `nano helloworld.py`

8.- Una vez dentro del editor de nano simplemente mandamos a imprimir el mensaje con:
```python
print("Hola Mundo")
```

9.- Guardamos los cambios del editor con `Ctrl + O` para guardar el archivo, presionamos `Enter` para confirmar el nombre del archivo y salimos de nano con `Ctrl + X`

10.- Ejecutamos el archivo con: `python3 helloworld.py`

11.- Una vez hechos estos cambios salimos del contenedor con `exit` y posteriormente procedemos a detener el contenedor con `docker stop ubuntu-container`

12.-Finalmente guardamos el contenedor en una nueva imagen con:
`docker commit ubuntu-container ubuntu-custom`
Nota: `ubuntu-container` sigue siendo el nombre del contenedor mientras que `ubuntu-custom` es el nombre que le asignamos a nuestra imagen y se pueden ver nuestras imágenes con `docker images`

## Procedimiento posterior al commit inicial

1.- Volvemos a iniciar y a acceder a nuestro contenedor `ubuntu-container` con los comandos previamente mencionados `docker start ubuntu-container` y luego `docker exec -it ubuntu-container bash`

2.- Una vez estando nuevamente en el contenedor procedemos a crear y editar un archivo de menú con opciones con extensión de bash simplemente con el comando de `nano menu.sh`

3.- El menú del archivo se edita y queda con esta estructura:
```bash
while true; do
    clear
    echo "Menú  Principal"
    echo "1. Ejecutar el archivo de Python"
    echo "2. Ejecutar prueba unitaria del archivo Python"
    echo "3. Mostrar la versión de Python utilizada"
    echo "4. Fecha y hora actual"
    echo "5. Mostrar el espacio en el disco disponible"
    echo "6. Salir"
    echo
    read -p "Elige una opción [1-6]: " opcion

    case $opcion in
        1)
            echo "Ejecutando el archivo Python..."
            python3 helloworld.py
            read -p "Presiona Enter para volver al menú..."
            ;;
        2)
            echo "Ejecutando prueba unitaria con Node/Mocha..."
            npx mocha
            read -p "Presiona Enter para volver al menú..."
            ;;
        3)
            echo "Versión actual de Python instalada:"
            python3 --version
            read -p "Presiona Enter para volver al menú..."
            ;;
        4)
            echo "Fecha y hora actual..."
            date
            read -p "Presiona Enter para volver al menú..."
            ;;
        5)
            echo "Espacio en disco disponible..."
            df -h
            read -p "Presiona Enter para volver al menú..."
            ;;
        6)
            echo "Saliendo del sistema. Que tengas un buen día..."
            exit 0
            ;;
        *)
            echo "Opción inválida, intenta de nuevo."
            read -p "Presiona Enter para continuar..."
            ;;
    esac
done
```

4.- Guardamos los cambios del editor con `Ctrl + O` para guardar el archivo, presionamos `Enter` para confirmar el nombre del archivo y salimos de nano con `Ctrl + X`
Nota: Las opciones del menú se caracterizan por su simplicidad, la primera ejecuta el archivo previamente creado, la tercera nos muestra la versión de Python que tenemos instalada, la cuarta nos da la fecha y la hora actuales, la quinta nos dice cuanto espacio tenemos disponible en nuestro disco y la sexta nos permite salir del menú. Cualquier otro carácter será invalido en la selección del menú.

5.- La opción destacable del menú es la segunda ya que es la prueba unitaria y es la que requiere más ejecución y envuelve la mayor complejidad. Lo primero que se ocupa es instalar tanto Node.js como npm con `apt install nodejs` y `apt install npm` respectivamente
Nota: Verificamos la instalación con `node -v` y `npm -v`

6.- Creamos un directorio de test con un archivo para la prueba unitaria listo para ser editado con nano con el comando: `nano test/testHelloWorld.js`

7.- El código de la prueba unitaria solamente verifica que el mensaje lanzado al ejecutar el archivo de helloworld.py sea estrictamente idéntico a ‘Hello World from Ubuntu’ y es el siguiente:
```JavaScript
const assert = require('assert');
const { exec } = require('child_process');

describe('Prueba de helloworld.py', function() {
  it('Debe de imprimir "Hello World from Ubuntu"', function(done) {
    exec('python3 helloworld.py', (error, stdout, stderr) => {
      if (error) {
        return done(error);
      }
      assert.strictEqual(stdout.trim(), 'Hello World from Ubuntu');
      done();
    });
  });
});
```
Nota: El contenido del archivo si es idéntico al que solicita la prueba por lo que al ejecutarla siempre pasará la aserción.

8.- Es importante también modificar el archivo package.json para que las pruebas trabajen con mocha. La estructura del archivo package.json luce así:
```JSON
{
  "name": "",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "directories": {
    "lib": "lib"
  },
  "scripts": {
    "test": "mocha"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

NOTA IMPORTANTE: Se intentó instalar mocha en el proyecto con `npm install mocha --save-dev` pero nunca se logró debido a que siempre marcaba error. Por más que se intentó desinstalar y reinstalar node y npm nunca se logró. Esta es la razón por la cual el menu.sh tiene configurada la ejecución de la prueba con `npx` en lugar de `npm` ya que así se descargará y ejecutará mocha por lo menos de manera temporal ya que hubo muchos problemas con su instalación global en el contenedor

9.- Finalmente damos los permisos de ejecución para poder ejecutar el script con el comando de:
 `chmod +x menu.sh`

10.- Sin nada más que modificar ya podemos ejecutar nuestro script sin problemas simplemente con:
`./menu.sh`

11.- En la ejecución podremos ver que el menú funciona con todas sus opciones correspondientes sin ningún problema y lanza la alerta si no se envía una opción válida.

12.- Una vez hechos estos cambios salimos del contenedor con `exit` y posteriormente procedemos a detener el contenedor con `docker stop ubuntu-container`

13.- IMPORTANTE: Se guarda este proyecto en una nueva imagen, pero manteniendo el nombre de la creada previamente solo que se le añade la etiqueta adicional de para diferenciarla de `ubuntu-custom`, la imagen creada en el proceso anterior. Esto se logra ejecutando el comando de: `docker commit ubuntu-container ubuntu-custom:v2`, así `ubuntu-custom` de diferencia con la etiqueta de `v2` pero mantiene su nombre para indicar que todo esto pertenece a la misma imagen aunque diferentes commits

Nota: Muchas gracias por leerme Marco!