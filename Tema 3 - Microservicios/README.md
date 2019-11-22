# Ejercicios Tema 3 - Microservicio
---

### 1. Realizar una aplicación básica que use express para devolver alguna estructura de datos del modelo que se viene usando en el curso.
 Yo en mi caso voy a usar `flask`, ya que en mi caso el proyecto lo estoy haciendo en Python 3.

~~~

from flask import Flask, jsonify, request

app = Flask(__name__)

# ------------------ ESTADO ------------------------------------------------#

# Ruta que devuelve status OK
@app.route('/', methods = ['GET'])
def inicio():
	return jsonify(status="OK"),200


# Ruta para el hito 4 (Contenedor Docker)
@app.route('/status', methods = ['GET'])
def status():
	return jsonify(status="OK"),200


~~~

---

### 2. Programar un microservicio en express (o el lenguaje y marco elegido) que incluya variables como en el caso anterior.

Como continuación del ejemplo anterior tenemos:

~~~

# Si usas el método devuelve o elimina una entrada con id = n
@app.route('/entrada/<n>', methods = ['GET','DELETE'])
def entrada(n):

	try:
		id = int(n)
	except:
		return '',400

	if request.method == 'DELETE':
		codigo = catalogo.delEntradaById(n)
		return '', codigo
	data = {}
	if request.method == 'GET':
		data = catalogo.getEntradaById(n)
	return data,200


~~~

---

### 3. Crear pruebas para las diferentes rutas de la aplicación.

Vamos a ver los test para las rutas anteriores.

~~~

#!/usr/bin/python
# -*- coding: utf-8 -*-
import sys
sys.path.append('../src/')

import pytest
import app


@pytest.fixture
def client():
    app.app.config['TESTING'] = True
    client = app.app.test_client()

    yield client

def test_status(client):
    rv = client.get('/')
    rv2 = client.get('/status')
    json_data = rv.get_json()
    json_data2 = rv2.get_json()
    assert json_data['status']=="OK" and rv.status_code == 200
    assert json_data['status']=="OK" and rv2.status_code == 200


def test_entrada(client):
    # Casos satisfactorios
    rv = client.put('/entrada', json={"entrada": [{"descripcion": "Esto es una" +
    "prueba en la base de datos estamos insertando datos en la base de datos para" +
    "probarla a ver como funciona bien o mal regular o peor",
    "evento": "AÑADIR ENTRADA", "precio": 23.1}]})
    rv2 = client.get('/entradas')
    json_data = rv2.get_json()
    id = json_data['entradas'][-1]['id']
    rv3 = client.get('/entrada/'+str(id))
    rv5 = client.post('/entrada/comprar/'+str(id), json={"propietario": "luisito@gmail.com"})
    rv6 = client.get('/entradas/propietario/luisito@gmail.com')
    # FALLO
    rv5F2 = client.post('/entrada/comprar/'+str(id), json={"propietario": "luisito@gmail.com"})

    rv4 = client.delete('/entrada/'+str(id))

    # Casos con fallos
    rvF = client.put('/entrada', json={})
    rv3F = client.delete('/entrada/-21')
    rv4F = client.get('/entrada/dds')
    rv5F1 = client.post('/entrada/comprar/'+str(id))
    rv5F3 = client.post('/entrada/comprar/'+str(id), json={"propietario": "luisito@gmail.com"})


    # Casos satisfactorios
    assert rv.status_code == 201
    assert rv2.status_code == 200
    assert rv3.status_code == 200
    assert rv4.status_code == 201
    assert rv5.status_code == 201
    assert rv6.status_code == 200



    # Casos con fallos
    assert rvF.status_code == 400
    assert rv3F.status_code == 404
    assert rv4F.status_code == 400
    assert rv5F1.status_code == 400
    assert rv5F2.status_code == 401
    assert rv5F3.status_code == 404

~~~



---

### 4. Experimentar con diferentes gestores de procesos y servidores web front-end para un microservicio que se haya hecho con antelación, por ejemplo en la sección anterior.

En mi caso para lanzar la aplicación uso `gunicorn` de la siguiente manera: `gunicorn -b :8080 app:app`.  
En este caso solo tengo que poner el puerto donde quiero que se despliegue y el nombre del fichero donde tengo flask.

---

### 5. Usar rake, invoke o la herramienta equivalente en tu lenguaje de programación para programar diferentes tareas que se puedan lanzar fácilmente desde la línea de órdenes.

Por ejemplo para invoke vamos a ver como lanzar o parar el servidor web con la aplicación.

~~~


# Tarea para iniciar el servicio
@task
def start(ctx):
    with ctx.cd('src/'):
        ctx.run("gunicorn -b :8080 app:app &")

# Tarea para parar el servicio
@task
def stop(ctx):
    ctx.run("pkill gunicorn")


~~~


---

**Todo esto se puede ver completo en el repositorio del [proyecto](https://github.com/iMiguel10/Proyecto-CC)**
