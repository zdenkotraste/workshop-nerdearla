#Usando una imagen de base de python para facilitarnos la creacion de la imagen
FROM python:3.7
#Creamos el directorio principal de trabajo
RUN mkdir /app
WORKDIR /app
#Copiamos nuestros archivos a nuestro directorio principal de trabajo
ADD . /app/
#Instalamos las dependencias de la aplicacion 
RUN pip install -r requirements.txt
#Creamos una variable para no tener que especificar el nombre del archivo .py en el cmd
RUN export FLASK_APP=app.py
#El puerto expuesto es el default de flask, el 5000
EXPOSE 5000
#El comando ejecutado sera python -m flask run --host 0.0.0.0, de esta forma podremos acceder de forma correcta al servicio
ENTRYPOINT ["python"]
CMD ["-m", "flask", "run", "--host", "0.0.0.0"]

#Prueba CI en el bootcamp
