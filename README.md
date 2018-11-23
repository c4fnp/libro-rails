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

## Diseñando la aplicación

1. Comencemos por los casos de uso que queremos soportar

    Los usuarios pueden actuar con dos perfiles diferentes dependiendo de su necesidad:

    - Guest
    - Host

    Cada perfil tiene diferentes requerimientos:

    - Guest
      - Login/Signup
      - Listar/Buscar lugares
      - Ver el detalle de un lugar
      - Crear reservas
      - Pagar reserva
      - Calificar un lugar después de haber finalizado una reserva
    - Host
      - Login/Signup
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

## Usuarios y autenticación

Para la autenticación vamos a usar una gema muy popular llamada [devise](https://github.com/plataformatec/devise).

Devise es muy flexible y nos provee con todo lo necesario para agregarle autenticación a nuestra aplicación. Después podemos modificar la configuración de devise para usar Oauth por ejemplo con Facebook, Github, Auth0, etc.

1. Instalación

    Agrega `gem 'devise', '~> 4.5'` a `Gemfile` y ejecuta `bundle install` en la terminal.

    > Después de hacer `bundle install`, te recomiendo verificar qué cambió en `Gemfile.lock` para que seas conciente de las dependencias que estás agregando al proyecto. Algunas personas suelen incluir gemas para solucionar problemas muy simples sin pensar en la cantidad de dependencias extra que estas gemas están agregando. Esto puede afectar el rendimiento de la aplicación pues el interprete de ruby va a tener que cargar en memoria estas dependencias extra.

1. Configuración de devise

    Ahora ejecuta `rails generate devise:install` en la terminal y sigue las instrucciones que el generador de devise nos muestra. Deben verse algo así:

    ```(txt)
    Running via Spring preloader in process 79685
          create  config/initializers/devise.rb
          create  config/locales/devise.en.yml
    ===============================================================================

    Some setup you must do manually if you haven't yet:

      1. Ensure you have defined default url options in your environments files. Here
        is an example of default_url_options appropriate for a development environment
        in config/environments/development.rb:

          config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

        In production, :host should be set to the actual host of your application.

      2. Ensure you have defined root_url to *something* in your config/routes.rb.
        For example:

          root to: "home#index"

      3. Ensure you have flash messages in app/views/layouts/application.html.erb.
        For example:

          <p class="notice"><%= notice %></p>
          <p class="alert"><%= alert %></p>

      4. You can copy Devise views (for customization) to your app by running:

          rails g devise:views

    ===============================================================================
    ```

    1. Agrega `config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }` a `config/environments/development.rb` y agrega un "TODO" a `config/environments/production.rb` para que nos acordemos de modificar la configuración de producción más adelante.

    1. Ya lo hicimos cuando creamos la página de home

    1. Agrega los "flash messages" a `app/views/layouts/application.html.erb`

    1. Por el momento no necesitamos modificar las vistas de autenticación que devise genera automáticamente. Lo haremos más adelante.

    **Para revisar:**

    - [flash messages](https://guides.rubyonrails.org/action_controller_overview.html#the-flash)
    - [generadores de rails](https://guides.rubyonrails.org/generators.html)

1. Creación del modelo usuario

    ```(bash)
    # generación del modelo
    rails generate devise User
    ```

1. Veamos los cambios que hemos hecho hasta el momento

    User.rb
    ```(ruby)
    class User < ApplicationRecord
      # Include default devise modules. Others available are:
      # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
      devise :database_authenticatable, :registerable,
            :recoverable, :rememberable, :validatable
    end
    ```

    El resto de cambios
    ```(bash)
    git add .
    git commit -m "Add devise and user model"
    git diff HEAD~
    ```

    **Para revisar:**

    - Configuración de devise `config/initializers/devise.rb`
    - Configuración de ambientes `config/environments/*.rb`
    - Internacionalización
      - `config/locales/devise.en.yml`
      - [I18N en rails](https://guides.rubyonrails.org/i18n.html)
    - Migraciones
  
## Migraciones

Las migraciones de la base de datos son parte fundamental de RoR. Con las migraciones podemos hacer que la estructura de la base de datos sea parte del código y adicionalmente garantizar que todos los desarrolladores que estén trabajando en el mismo proyecto puedan tener la misma estructura en todo momento.

Creemos la tabla usuario en la base de datos:

```(bash)
rails db:migrate
```

Debes ver algo como esto:

```(txt)
== 20181123203504 DeviseCreateUsers: migrating ================================
-- create_table(:users)
  -> 0.0011s
-- add_index(:users, :email, {:unique=>true})
  -> 0.0006s
-- add_index(:users, :reset_password_token, {:unique=>true})
  -> 0.0007s
== 20181123203504 DeviseCreateUsers: migrated (0.0026s) =======================
```

Mira el archivo `db/schema.rb`

**Para revisar:**

- [Documentación oficial de migraciones](https://guides.rubyonrails.org/active_record_migrations.html)

## rails console

1. Verifiquemos que todo funciona

```(bash)
rails s
```

Ve a `localhost:3000`

No hay nada nuevo... What?

Ve a la terminal y ejecuta `rails routes`. Debes ver algo como esto:

```(txt)
rails routes
                   Prefix Verb   URI Pattern                                                                              Controller#Action
         new_user_session GET    /users/sign_in(.:format)                                                                 devise/sessions#new
             user_session POST   /users/sign_in(.:format)                                                                 devise/sessions#create
     destroy_user_session DELETE /users/sign_out(.:format)                                                                devise/sessions#destroy
    new_user_registration GET    /users/sign_up(.:format)                                                                 devise/registrations#new
```

> Utiliza `rails routes` cuando quieras verificar las rutas de las que dispones en la aplicación. Puede ser muy util cuando tienes una aplicación con muchos recursos y rutas.

Ve a `localhost:3000/users/sign_up`

Ahora vamos a la terminal y ejecuta `rails c`. Este comando va a abrir la consola de rails. Es básicamente `irb` pero con funciones adicionales para aplicaciones rails y además todas las gemas y archivos de tu aplicación cargados.

Haz lo siguiente en la consola de rails:

```(ruby)
User.count
User.all
User.first
simon = User.create(email: "simon@mail.com", password: "password")
User.count
User.all
simon2 = User.new(email: "simon@mail.com", password: "password")
simon2.valid?
simon2.errors
simon2.save!
simon2.email = "simon2@mail.com"
simon2.valid?
simon2.save!
User.count
User.all
User.first
User.where(email: "simon@mail.com")
exit
```

**Para revisar:**

- flash messages
- rails console