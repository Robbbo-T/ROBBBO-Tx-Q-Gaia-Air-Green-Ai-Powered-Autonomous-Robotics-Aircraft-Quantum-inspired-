# Usar una imagen base con Python 3.8 y Node.js
FROM mcr.microsoft.com/vscode/devcontainers/python:3.8

# Instalar Node.js LTS y npm
RUN apt-get update && \
    curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs

# Instalar herramientas de desarrollo y dependencias
RUN apt-get install -y \
    build-essential \
    cmake \
    gdb \
    lldb \
    clang \
    clang-format \
    clang-tidy \
    cppcheck \
    doxygen \
    graphviz \
    git \
    wget \
    libboost-all-dev \
    libssl-dev

# Instalar Google Test (gtest)
RUN apt-get install -y libgtest-dev && \
    cd /usr/src/gtest && \
    cmake . && \
    make && \
    mv lib/*.a /usr/lib

# Instalar Docker dentro del contenedor (opcional)
# RUN apt-get install -y docker.io

# Actualizar pip y configurar directorio de trabajo
RUN pip install --upgrade pip
WORKDIR /workspace

# Copiar archivos de requisitos (si existen)
COPY requirements.txt /workspace/requirements.txt
COPY package.json /workspace/package.json
COPY package-lock.json /workspace/package-lock.json

# Instalar dependencias de Python y Node.js
RUN pip install -r requirements.txt
RUN npm install

# Exponer puertos necesarios
EXPOSE 3000 8000

# Configurar usuario no root
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME && \
    useradd --uid $USER_UID --gid $USER_GID -m $USERNAME && \
    chown -R $USERNAME:$USERNAME /workspace && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME

USER $USERNAME
