# Protocolos-de-Internet
#**Consigna del TP2 A. Desarrollo de Cliente - Servidor bajo Arquitectura Rest**

**1- Analizar la aplicación Flask básica. Explicar el funcionamiento de app = Flask(name), @app.route("/"), jsonify() y el modo debug=True, tomando como base Prueba_Flask_Routes_01.py.**

  _Prueba_Flask_Routes_01.py._
  
  app = Flask(name) crea una instancia en la que se inicia un servidor de Flask. Name le indica a Flask donde está el módulo inicial y ubicar rutas relativas correctamente.
  @app.route() es un endpoint, que tiene GET como método por defecto, es decir, devuelve texto.
  @app.route('/')
  def index():
      return "Hola"
  
  La función jsonify() convierte objetos Python en una respuesta HTTP JSON válida. Sin	está, el return puede fallar.
  El modo debug=True activa el modo desarrollo, lo que activa el auto-reload (El server se 	reinicia cuando guardo los cambios).



**2- Implementar una API REST de sensores.
Desarrollar endpoints para listar sensores, consultar un sensor por id, crear un sensor, modificarlo y eliminarlo usando métodos GET, POST, PUT y DELETE.**

**3- Corregir y mejorar el manejo de IDs
Revisar el acceso directo a listas mediante sensors[id] y reemplazarlo por una búsqueda segura por campo id, devolviendo error 404 si no existe.**

**4- Comparar GET vs POST en Flask
Implementar dos rutas de login: una con parámetros por URL y otra con formulario POST. Explicar diferencias de visibilidad, seguridad básica y uso correcto de request.args y request.form.
**
**5- Persistir lecturas de sensores en SQLite
Crear una tabla lectura_sensores con campos como co2, temp, hum, fecha, lugar, altura, presion, presion_nm y temp_ext, siguiendo la estructura usada en sensores_rx.py.
**
**6- Simular capturas de sensores ambientales
Generar lecturas aleatorias de CO₂, temperatura y humedad; almacenarlas en la base de datos y permitir configurar cantidad de capturas e intervalo entre mediciones.
**
**7- Integrar datos externos de clima
Usar una función similar a geo_latlon() para obtener temperatura exterior, presión y humedad desde una API climática, y relacionar esos datos con las lecturas internas.**

**8- Explicar las razones por la cual lo anterior es una Arquitectura CLiente Servidor Rest. Indicar si se cumplen los requisitos. Explicar dónde se definen el cliente y el servidor. Qué elementos faltan**.

**9- Comparar este modelo con el desarrollado en el TP1.**

**10- Desarrollar cliente visual WebSocket
Crear una interfaz web que se conecte a un servidor WebSocket, muestre estado de conexión, permita enviar mensajes y visualice respuestas en tiempo real, según el notebook Cliente_Servidor_Websockets_R2.ipynb.**
