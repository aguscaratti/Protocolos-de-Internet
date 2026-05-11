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

La respuesta a esta pregunta está en el archivo consigna_2_3_5_TP2.py

**3- Corregir y mejorar el manejo de IDs
Revisar el acceso directo a listas mediante sensors[id] y reemplazarlo por una búsqueda segura por campo id, devolviendo error 404 si no existe.**

El código consigna_2_3_5_TP2.py corrige y maneja los IDs de manera correcta; pero además corregimos parte del código Prueba_Flask_Routes_01.py de la siguiente manera para cumplir la consigna:

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

La respuesta a esta pregunta está en el archivo consigna_2_3_5_TP2.py

**6- Simular capturas de sensores ambientales
Generar lecturas aleatorias de CO₂, temperatura y humedad; almacenarlas en la base de datos y permitir configurar cantidad de capturas e intervalo entre mediciones.**

La respuesta a esta pregunta está en el archivo consigna_6_7_TP2.py

**7- Integrar datos externos de clima
Usar una función similar a geo_latlon() para obtener temperatura exterior, presión y humedad desde una API climática, y relacionar esos datos con las lecturas internas.**

La respuesta a esta pregunta está en el archivo consigna_6_7_TP2.py

**8- Explicar las razones por la cual lo anterior es una Arquitectura CLiente Servidor Rest. Indicar si se cumplen los requisitos. Explicar dónde se definen el cliente y el servidor. Qué elementos faltan**.

Es una Arquitectura Cliente-Servidor Rest porque el cliente se encarga de la interfaz y el servidor de la lógica y los datos. A su vez, porque tiene una interfaz uniforme, se usan URIs y se usan mediante el protocolo HTTP (GET, POST, DELETE). Es stateless, cada solicitud del cliente al servidor debe contener la información necesaria para entenderla, sin que el servidor guarde contexto de solicitudes anteriores. 
	Los elementos que le faltan son: 
- HATEOAS: Incluir hipervínculos en las respuestas JSON para guiar al cliente sobre qué otras acciones puede realizar.
- Sistema por capas: una arquitectura REST debe permitir capas intermedias sin que el cliente lo note. 
- Caché:  para almacenar los datos solicitados con frecuencia. 

**9- Comparar este modelo con el desarrollado en el TP1.**
La diferencia principal entre el TP1 y el TP2 está en que en el TP1 trabajamos en las capas de Transporte y Red mediante el uso de sockets TCP/UDP y también RAW Sockets para entender como viajan los datos bit a bit. En el segundo trabajamos sobre la capa de Aplicación, donde implementamos una Arquitectura REST que es Stateless, y usamos el protocolo HTTP para gestionar URIs. El TP1 nos ayudó a entender como es la comunicación a un nivel más crudo, mientras que el TP2 nos ayuda a ver los servicios web. 




**10- Desarrollar cliente visual WebSocke
Crear una interfaz web que se conecte a un servidor WebSocket, muestre estado de conexión, permita enviar mensajes y visualice respuestas en tiempo real, según el notebook Cliente_Servidor_Websockets_R2.ipynb.**

Esta consigna está cumplida por el servidor "server_ws.js" y el cliente "cliente_wsp.py"
