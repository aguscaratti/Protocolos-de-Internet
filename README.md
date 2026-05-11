# Protocolos-de-Internet
#**Consigna del TP2 A. Desarrollo de Cliente - Servidor bajo Arquitectura Rest**

**1- Analizar la aplicación Flask básica. Explicar el funcionamiento de app = Flask(name), @app.route("/"), jsonify() y el modo debug=True, tomando como base Prueba_Flask_Routes_01.py.**

  _Prueba_Flask_Routes_01.py._
  
  app = Flask(name) crea una instancia en la que se inicia un servidor de Flask. Name le indica a Flask donde está el módulo inicial y ubicar rutas relativas correctamente.
  @app.route() es un endpoint, que tiene GET como método por defecto, es decir, devuelve texto.
 ```python
app = Flask(__name__)

@app.route('/')
def index():
    return "Hola"
 ```
 La función jsonify() convierte objetos Python en una respuesta HTTP JSON válida. Sin	está, el return puede fallar.
 El modo debug=True activa el modo desarrollo, lo que activa el auto-reload (El server se 	reinicia cuando guardo los cambios).



**2- Implementar una API REST de sensores.
Desarrollar endpoints para listar sensores, consultar un sensor por id, crear un sensor, modificarlo y eliminarlo usando métodos GET, POST, PUT y DELETE.**

```python
from flask import Flask,jsonify
app = Flask(__name__)
sensors = [{'id':"0",
            'co2':"800",
            'temp':"23.0",
            'hum':"77.1",
            'fecha':"22/3/2021"},
           {'id': "1",
            'co2': "801",
            'temp': "24.0",
            'hum': "78.1",
            'fecha': "23/3/2021"}
          ]

@app.route('/')
def index():
    return "Hola"

@app.route("/sensors", methods=['GET'])
def get():
    return jsonify ({'Sensors':sensors})

@app.route("/sensors/<int:id>", methods=['GET'])
def get_measure(id):
    return jsonify ({'Sensors':sensors[id]})

@app.route("/sensors", methods=['POST'])
def create():
    sensor = {'id': "2",
                'co2': "800",
                'temp': "23.0",
                'hum': "77.1",
                'fecha': "22/3/2021"}
               
    sensors.append(sensor)
    return jsonify ({'Created':sensor})

@app.route("/sensors/<int:id>", methods=['PUT'])
def sensor_update(id):
    sensors[id]['fecha'] = "22/9/2021"
    return jsonify ({'Sensor':sensors[id]})

@app.route("/sensors/<int:id>", methods=['DELETE'])
def sensor_delete(id):
    sensors.remove(sensors[id])
    return jsonify ({'result':True})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
```

**3- Corregir y mejorar el manejo de IDs
Revisar el acceso directo a listas mediante sensors[id] y reemplazarlo por una búsqueda segura por campo id, devolviendo error 404 si no existe.**

Antes para buscar ID, se buscaba por posición. 
```python
`@app.route("/sensors/<int:id>", methods=['GET'])
def get_measure(id):
  	 return jsonify ({'Sensors':sensors[id]})


@app.route("/sensors/<int:id>", methods=['GET'])
def get_measure(id):
   for sensor in sensors:
       if sensor['id'] == str(id):
           return jsonify({'Sensor': sensor})
   return jsonify({'error': 'Sensor no encontrado'}), 404
```

**4- Comparar GET vs POST en Flask
Implementar dos rutas de login: una con parámetros por URL y otra con formulario POST. Explicar diferencias de visibilidad, seguridad básica y uso correcto de request.args y request.form.**

  - GET: Los datos enviados por el usuario viajan en la URL mediante parámetros, por lo que quedan visibles en la barra del navegador,            pueden guardarse en el historial y copiarse o compartirse fácilmente.
```python

@app.route("/login_get", methods=["GET"])
def login_get():

    nombre = request.args.get("nombre") #lee parámetros de URL
    clave = request.args.get("clave")

    if nombre == "victoria" and clave == "abc":
        return f"Hola {nombre} con GET"

    return "Usuario o clave incorrectos", 401


@app.route("/")
def index():
    return """
    <form action="/login" method="get">
        Nombre: <input type="text" name="nombre"><br>
        Clave: <input type="password" name="clave"><br>
        <input type="submit">
    </form>
    """
```

  - POST: Se emplea para enviar datos al servidor, como en formularios de registro, inicio de sesión o subida de archivos.  Los datos no           son visibles en la URL, lo que lo hace más seguro para información confidencial (aunque no cifrado por defecto). No tiene                límites prácticos de tamaño y los resultados no se almacenan en caché ni pueden marcarse fácilmente.
```python

@app.route("/login_post", methods=["POST"])
def login_post():

    nombre = request.form.get("nombre")
    clave = request.form.get("clave")

    if nombre == "victoria" and clave == "abc":
        return f"Hola {nombre} con POST"

    return "Usuario o clave incorrectos", 401


@app.route("/")
def index():
    return """
    <form action="/login" method="post">
        Nombre:<input type="text" name="nombre"><br><br>
        Clave:<input type="password" name="clave"><br><br>
        <input type="submit">
    </form>
    """
```

**5- Persistir lecturas de sensores en SQLite
Crear una tabla lectura_sensores con campos como co2, temp, hum, fecha, lugar, altura, presion, presion_nm y temp_ext, siguiendo la estructura usada en sensores_rx.py.**



**6- Simular capturas de sensores ambientales
Generar lecturas aleatorias de CO₂, temperatura y humedad; almacenarlas en la base de datos y permitir configurar cantidad de capturas e intervalo entre mediciones.**

**7- Integrar datos externos de clima
Usar una función similar a geo_latlon() para obtener temperatura exterior, presión y humedad desde una API climática, y relacionar esos datos con las lecturas internas.**

**8- Explicar las razones por la cual lo anterior es una Arquitectura CLiente Servidor Rest. Indicar si se cumplen los requisitos. Explicar dónde se definen el cliente y el servidor. Qué elementos faltan**.

**9- Comparar este modelo con el desarrollado en el TP1.**

**10- Desarrollar cliente visual WebSocket
Crear una interfaz web que se conecte a un servidor WebSocket, muestre estado de conexión, permita enviar mensajes y visualice respuestas en tiempo real, según el notebook Cliente_Servidor_Websockets_R2.ipynb.**
