FROM alpine:latest

RUN apk --no-cache --update add ca-certificates python3

# Install requirements.txt
COPY requirements.txt /tmp
RUN python3 -m pip install -r /tmp/requirements.txt

ADD src/ /data
WORKDIR /data

# Create a non-priviledged system user
RUN adduser -S www
USER www

CMD gunicorn -b 0.0.0.0 server:app
