FROM mcr.microsoft.com/devcontainers/python:3.11

# Instalar dependencias del sistema necesarias para C++ y PyCUDA
RUN apt-get update && apt-get install -y \
    build-essential \
    python3-dev \
    libboost-all-dev \
    && rm -rf /var/lib/apt/lists/*

# Copiar e instalar requerimientos de Python
COPY requirements.txt /tmp/pip-tmp/
RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp