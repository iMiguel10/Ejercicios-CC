# Ejercicios Tema 2 - Desarrollo basado en pruebas
---

### 1. Instalar alguno de los entornos virtuales de node.js (o de cualquier otro lenguaje con el que se esté familiarizado) y, con ellos, instalar la última versión existente, la versión minor más actual de la 4.x y lo mismo para la 0.11 o alguna impar (de desarrollo).

En este caso instalé virtualenv para python y fuera del entorno virtual tengo python 2 y dentro tengo python 3.

![virtualenv](https://github.com/iMiguel10/Ejercicios-CC/blob/master/Tema%202%20-%20Desarrollo%20basado%20en%20pruebas/virtualenv.PNG)

---

### 2. Crear una descripción del módulo usando `package.json` En caso de que se trate de otro lenguaje, usar el método correspondiente.

![package-json](https://github.com/iMiguel10/Ejercicios-CC/blob/master/Tema%202%20-%20Desarrollo%20basado%20en%20pruebas/package-json.png)

---

### 4. Añadir tests para una nueva funcionalidad, probar que falla y escribir el código para que no lo haga. A continuación, ejecutarlos desde mocha (u otro módulo de test de alto nivel), usando descripciones del test y del grupo de test de forma correcta.

Para la realización de este ejercicio se ha usado PyTest. Además los test escritos son sobre el proyecto de la asignatura.

~~~

#!/usr/bin/python
# -*- coding: utf-8 -*-

import pytest
import sys
sys.path.append('../src/')
from generadorentradasPDF import Documento

@pytest.fixture
def documento():
    '''Devuelve una instancia del Documento con el contenido correcto'''
    datos = {
        "id": 12232323,
        "evento": "Concierto Pablo Alborán",
        "precio": 49.99,
        "propietario": "probando@correo.es",
        "descripcion": "Lorem ipsum dolor sit amet consectetur adipiscing elit, augue enim nulla sodales vulputate ad, lacus himenaeos nostra ante cubilia ut. Penatibus arcu semper ultricies viverra platea netus cubilia parturient per turpis class sollicitudin habitasse, sem primis tincidunt libero duis eros erat nostra luctus dis sociosqu ut senectus, quis dui purus lectus nunc mattis ornare hac ad id convallis enim. Praesent accumsan luctus pharetra congue nostra vitae aenean nascetur, sem curabitur quam tristique massa inceptos."
    }
    return Documento(datos)

@pytest.fixture
def documento_error():
    '''Devuelve una instancia del Documento con el contenido erroneo'''
    datos = {"as":"as"}
    return Documento(datos)

def test_generarQR(documento,documento_error):
    '''Comprobar que se genera el QR correctamente'''
    assert documento.generaQR() != False, "Ha fallado la creación del QR"
    assert documento_error.generaQR() != False,  "Ha fallado la creación del QR"

def test_generarPDF(documento,documento_error):
    '''Comprobar que se genera PDF correctamente'''
    assert documento.generarPDF() != False, "Los datos son incorrectos, deberían tener un formato adecuado"
    assert documento_error.generarPDF() == False, "Los datos son incorrectos, deberían tener un formato adecuado"

~~~

[**Test**](https://github.com/iMiguel10/Proyecto-CC/blob/master/test/test_generadorentradasPDF.py)

---

### 5. Ejercicio: Añadir integracion contínua

Usando Travis CI hemos incorporado la integracion contínua en el repositorio del proyecto de la asignatura.

[**Proyecto: Gestión de entradas**](https://github.com/iMiguel10/Proyecto-CC)

---
