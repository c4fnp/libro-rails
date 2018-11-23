# Curso de rails

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

## MVC

Rails es un framework de desarrollo web basado en MVC (Modelo, Vista, Controlador).

<!-- TODO: Explicat MVC -->

## Estructura de un proyecto rails

- config
  - routes.rb
  - application.rb
  - database.yml
  - storage.yml
- models
- db
  - migrations
  - seeds
- views
- controllers
- mailers
- jobs
- channels
- assets
  - images
  - javascripts
  - images
- test
- vendor
- .gitignore

## Hello World

1. Crea el primer commit

    El proyecto ya incluye un .gitgnore que asegura que solo vamos a incluir los archivos necesario en el repositorio.

    ```(bash)
    git add .
    git commit -m "Initial commit"
    ```
1. Creemos la página principal

    ```(bash)
    touch app/controllers/pages_controller.rb
    mkdir app/views/pages
    touch app/views/pages/home.html.erb
    ```

    En `app/config/routes.rb`
    ```(ruby)
      get "/home" to: "pages#home"
    ```

    En `app/controllers/pages_controller.rb`
    ```(ruby)
    class PagesController < ApplicationController
      def home
        render "home"
      end
    end
    ```

    En `app/views/pages/home.html.erb`
    ```(erb)
    <h1>Airdnd</h1>
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Ex voluptatibus aliquam cumque itaque non odit, praesentium autem velit impedit. Laudantium incidunt optio harum ab quos recusandae inventore at corrupti maxime.
    </p>
    ```

    Remueve el `render "home"` del `pages_controller`

    Usemos home como la página principal

    ```(ruby)
    - get "/home" to: "pages#home"
    + root "pages#home"
    ```

    Lo que aprendimos:
    - ERB
    - [rutas](https://guides.rubyonrails.org/routing.html)
      - get
      - root
    - "Convention over configuration"

1. Comencemos por los casos de uso que queremos soportar

    Los usuarios pueden actuar con dos perfiles diferentes dependiendo de su necesidad:

    - Guest
    - Host

    Cada perfil tiene diferentes requerimientos:

    - Guest
      - Listar/Buscar lugares
      - Ver el detalle de un lugar
      - Crear reservas
      - Pagar reserva
      - Calificar un lugar después de haber finalizado una reserva
    - Host
      - Crear un lugar
      - Listar reservas
      - Listar pagos
      - Calificar a un Guest después de haber finalizado una reserva

      ![casos de uso Airdnd](/assets/casos_de_uso_airdnd.png)
    <!-- TODO: hacer un diagrama de casos de uso bonito -->
    <!-- TODO: Remover lo de solicitud de reserva. Hace mas complicada la app. Simplemente reservar de una. -->

1. Ahora pensemos en los modelos que necesitamos y cómo van a estar relacionados

    Modelos necesarios:

    - User
    - Place <!--TODO: renombrar listing a place -->
    - Booking
    - Payment
    - UserReview
    - PlaceReview

    ![modelos y relaciones Airdnd](/assets/entidad_relacion_airdnd.png)
    <!-- TODO: hacer un diagrama de entidad relacion bonito -->
    <!-- TODO: Usar los nombres correctos -->

