FROM python:3.9-alpine

RUN apk add --update curl gcc libffi-dev build-base icu-dev \
        && rm -rf /var/cache/apk/*

ENV PYTHONUNBUFFERED=1 APP_HOME=/microservice/ CONFIGMAP_FILE="$APP_HOME"config-docker.yml

RUN mkdir $APP_HOME && adduser -S -D -H python && chown -R python $APP_HOME
WORKDIR $APP_HOME

ADD requirements.txt $APP_HOME
RUN pip install --upgrade pip && pip install -r requirements.txt

ADD . $APP_HOME

EXPOSE 8080
USER python

CMD ["gunicorn", "--worker-class", "gevent", "--workers", "1", "--log-level", "INFO", "--bind", "0.0.0.0:8080", "manage:app"]