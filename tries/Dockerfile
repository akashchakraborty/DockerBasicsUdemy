FROM ubuntu
RUN apt-get update && apt-get -y install python3 python3-setuptools python3-dev build-essential python3-pip python3-mysqldb
RUN pip3 install flask flask-mysql

COPY app.py /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run