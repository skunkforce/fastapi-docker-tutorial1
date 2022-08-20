# FastAPI Docker Tutorial

1. Step
Installieren von [Docker Desktop](https://www.docker.com/get-started/)

1. Step
Neues Github Repo initialisieren - hierbei auf die [https://wiki.gruppe.ai/index.php/Guidelines:Namespaces|aiNamingconvention] achten.

1. Step
Repo in einen beliebigen Develompentordner auf der eigenen Maschine clonen. 

1. Step
Mit ```python3.<x> -m venv env``` eine virtuelle Arbeitsumgebung anlegen und mit ```.\env\Scripts\activate```(Windows) bzw. ```source ./env/bin/activate``` die virtuellen Umgebungsvariablen laden. ```<x>``` steht hierbei für die zu verwendende Python-Version.

1. Step
Mit ```pip install fastapi pydantic uvicorn``` die notwendigen Basispakete für ein einfaches FastAPI Projekt installieren. Im Anschluss mit ```pip freeze > requirements.txt``` die installierten Pakete in der `requirements.txt` abspeichern.

1. Step
Eine neue Datei namens ```Dockerfile``` anlegen und mit folgendem Inhalt füllen:
    ```Docker
    FROM python:3.<x>
    WORKDIR /code
    COPY ./requirements.txt /code/requirements.txt
    RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
    COPY ./app /code/app
    CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
    ```
    Erklärung dieses Dockerfiles findet sich auf der Seite von [Tiangolo](https://fastapi.tiangolo.com/deployment/docker/?h=docker#dockerfile).

1. Step
Docker-Image erstellen mit dem Befehl ```docker image build -t <image-name> .``` wobei der Imagename selbst gewählt werden sollte. Zwar kann auch das Dockerfile einen beliebigen Namen tragen, doch halten wir uns der einfachheit an den Namen Dockerfile.

    -> Veränderungen am Code muss das Image neu erstellt werden!

1. Step
Jetzt kann entweder über Docker-Desktop das Image gestartet werden, oder über die CMD mit dem Befehl `docker run -d -p <Port des Hostes>:80 <image-name>`.
```-d``` kennzeichnet den Detached-Mode. ```-p``` führt das Portmapping vom Containerinternen Port 80 auf einen beliebigen Host-Port aus. Hierbei muss darauf geachtet werden, dass kein weiterer Service diesen Port bereits innehat. Der Service lässt sich nun über den Browser aufrufen unter der ```<Host-IP>:<Port des Hostes>```. Auch für andere Teilnehmer im Netzwerk ist der Service erreichbar.

