# Configuración y creación del proyecto

## Configuración

1. Instalar Ruby (2.5.1)

1. Instalar sqlite3

    Verificar con `sqlite3 --version`

1. Instalar rails `gem install rails -v 5.2.1`
  
    Esto va a tomar varios minutos.
    Una vez terminado verifica usando `rails -v` y deberías ver `Rails 5.2.1`

## Creación del proyecto

1. `$ rails new airdnd`

    Este paso también va a tomar varios minutos.

    - Genera la estructura básica de un proyecto de rails
    - Inicializa un repositorio de git

1. Prueba que el proyecto quedó correctamente configurado

    ```(bash)
    cd airdnd
    rails s
    ```

    Abre un navegador y ve a `localhost:3000`

    Debes ver una página que dice "Yay! You’re on Rails!"
