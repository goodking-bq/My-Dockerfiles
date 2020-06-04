FROM alpine


# 设置时区为上海
ENV TZ "Asia/Shanghai"

RUN mkdir $HOME/.pip &&\
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories 

COPY ./pip.conf /root/.pip/pip.conf
COPY ./.pydistutils.cfg /root/.pydistutils.cfg
# INSTALL PYTHON3

RUN apk add --no-cache --update \
    python3 \
    python3-dev \
    py3-pip \
    build-base \
    libffi-dev \
    openssl-dev \
    tzdata \
    busybox-extras \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo '$TZ' > /etc/timezone
 
    
RUN pip3 install --no-cache-dir pipenv

ENV DEBIAN_FRONTEND noninteractive
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
# Python, don't write bytecode!
ENV PYTHONDONTWRITEBYTECODE 1



# -- Install Application into container:
RUN set -ex && mkdir /app

WORKDIR /app

# -- Adding Pipfiles
ONBUILD COPY Pipfile Pipfile
ONBUILD COPY Pipfile.lock Pipfile.lock

# -- Install dependencies:
ONBUILD RUN set -ex && pipenv install --clear --deploy --system

# --------------------
# - Using This File: -
# --------------------

# FROM kennethreitz/pipenv

# COPY . /app

# -- Replace with the correct path to your app's main executable
# CMD python3 main.py
