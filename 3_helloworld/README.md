# Hello World

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
