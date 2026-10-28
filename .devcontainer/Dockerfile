FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

# Evitar prompts interactivos durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Instalar Python, pip, herramientas de compilación y Git
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    build-essential \
    git \
    && rm -rf /var/lib/apt/lists/*

# Crear y activar un entorno virtual dentro del contenedor
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Copiar e instalar requerimientos
COPY requirements.txt /tmp/pip-tmp/
RUN pip install --disable-pip-version-check --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp