# Diseñando la aplicación

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