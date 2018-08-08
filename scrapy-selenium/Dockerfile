FROM python:3.6.2-alpine

RUN apk update
RUN apk add --no-cache --virtual .build-deps \
    g++ \
    gcc \
    build-base \
    libxml2-dev \
    libffi-dev \
    libxslt-dev \
    openssl-dev

RUN pip install \
    --no-cache-dir \
    bottle==0.12.13 \
    requests==2.19.1 \
    Scrapy==1.5.0 \
    python-dateutil==2.6.1 \
    urllib3==1.21.1 \
    Pillow==4.2.1 \
    pytesseract==0.1.7 \
    selenium==3.0.2

RUN apk del .build-deps
