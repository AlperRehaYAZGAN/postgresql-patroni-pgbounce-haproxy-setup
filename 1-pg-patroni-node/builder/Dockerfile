FROM postgres:14.4-alpine

RUN apk add --no-cache --virtual .build-deps g++ python3-dev libffi-dev openssl-dev && \
    apk add --no-cache --update python3

RUN apk add py3-pip
RUN pip3 install --upgrade pip setuptools
RUN pip3 install wheel

COPY ./requirements.txt /requirements.txt
RUN pip3 install -r /requirements.txt

COPY ./pgtune-postgres.conf /config/postgresql.conf
COPY ./pg_hba.conf /config/pg_hba.conf
RUN chown -R postgres:postgres /config

COPY ./patroni.yml /patroni.yml
ENTRYPOINT ["patroni", "patroni.yml"]