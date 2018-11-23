# Modelos y relaciones

1. Comencemos por crear el modelo de Lugares (Places).

    ```(bash)
    rails g model Place \
      country:string \
      city:string \
      price_per_night:decimal \
      bedrooms:integer \
      bathrooms:integer \
      guests_capacity:integer \
      user:references
    ```

    Revisemos los archivos que fueron generados y vayamos a la migración:

    ```(ruby)
    class CreatePlaces < ActiveRecord::Migration[5.2]
      def change
        create_table :places do |t|
          t.string :country
          t.string :city
          t.decimal :price_per_night
          t.integer :bedrooms
          t.integer :bathrooms
          t.integer :guests_capacity
          t.references :user, foreign_key: true


          t.timestamps
        end
      end
    end

    ```

    Por convención, si queremos hacer referencia a otro modelo, rails va a generar una llave foranea a la tabla que queremos referenciar con el nombre esa tabla y el sufijo `_id`. Por ejemplo, en este caso se va a generar el campo `user_id` y se va a usar como llave foranea la tabla `users`. Sin embargo, en este caso el nombre `user` no hace tanto sentido. Un lugar (place) no tiene un user, tiene un dueño (owner), así que vamos a modificar esa referencia antes de crear la tabla en la base de datos. Haz lo siguiente:

    ```(ruby)
    # Remueve esta línea
    t.references :user, foreign_key: true
    # Agrega esta a cambio
    t.references :owner, index: true, foreign_key: { to_table: :users }
    ```

    Ahora sí ejecuta la migración con `rails db:migrate` y fijate en el cambio en `schema.rb`:

    ```(ruby)
    create_table "places", force: :cascade do |t|
        t.string "country"
        t.string "city"
        t.decimal "price_per_night"
        t.integer "bedrooms"
        t.integer "bathrooms"
        t.integer "guests_capacity"
        t.integer "owner_id"
        t.datetime "created_at", null: false
        t.datetime "updated_at", null: false
        t.index ["owner_id"], name: "index_places_on_owner_id"
      end
    ```

    Se creó la tabla `places` con el campo `t.integer "owner_id"` y un índice que va a actuar como llave foranea `t.index ["owner_id"], name: "index_places_on_owner_id"`.

    Para terminar agreguemos la relación en el modelo usuario:

    ```(ruby)
    class User < ApplicationRecord
      # Include default devise modules. Others available are:
      # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
      devise :database_authenticatable, :registerable,
            :recoverable, :rememberable, :validatable

      # Agrega esta línea
      has_many :places, foreign_key: "owner_id", class_name: "Place"
    end
    ```

1. Probemos los cambios que hicimos en la consola de rails con `rails c`:

```(ruby)
Place.all
uu = User.first
uu.places
# Place Load (0.6ms)  SELECT  "places".* FROM "places" WHERE "places"."owner_id" = ? LIMIT ?  [["owner_id", 1], ["LIMIT", 11]]
```
