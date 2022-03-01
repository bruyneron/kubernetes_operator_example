FROM python:3.8-slim-buster

WORKDIR /python-docker

COPY requirements.txt requirements.txt
COPY flask_app.py app.py
RUN pip3 install -r requirements.txt


CMD [ "python3", "app.py"]

