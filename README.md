<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

<p align="center">
  <a href="https://hub.docker.com/r/astarthean/docker-graphql" target="_blank">
    <img src="https://img.shields.io/docker/v/astarthean/docker-graphql/latest" alt="Docker Image Version" />
  </a>
  <a href="https://github.com/Astarthean/docker-graphql/issues" target="_blank">
    <img src="https://img.shields.io/github/issues/Astarthean/docker-graphql" alt="GitHub Issues" />
  </a>
  <a href="LICENSE" target="_blank">
    <img src="https://img.shields.io/badge/license-UNLICENSED-blue.svg" alt="License" />
  </a>
</p>

<p align="center">
  API GraphQL construida con NestJS implementando Docker y CI/CD automatizado
</p>

## 📦 Características principales

- **GraphQL API**: Incluye un servidor GraphQL accesible en `/graphql`.
- **Dockerizado**: Listo para ejecutar en cualquier entorno compatible con Docker.
- **CI/CD Automatizado**: Utiliza GitHub Actions para construir y publicar imágenes Docker automáticamente cuando hay cambios en la rama `main`.


## 🚀 Primeros pasos

### Con Docker
```bash
# Ejecutar última versión estable
docker run -p 3000:3000 astarthean/docker-graphql:latest

# Acceder al playground GraphQL
http://localhost:3000/graphql
```

### Desarrollo Local
```bash
# Clonar repositorio
git clone https://github.com/Astarthean/docker-graphql.git
cd docker-graphql

# Instalar dependencias
yarn / yarn install

# Iniciar en modo desarrollo
yarn start:dev

# Crear imagen de docker
docker build -t docker-graphql:dev .
docker image ls

# Iniciar contenedor
docker run -p 3000:3000 docker-graphql:dev || docker run -p 3000:3000 IMAGE_ID

# Acceder al playground GraphQL
http://localhost:3000/graphql
```

### Uso de la API
```bash
{
  hello
  todos {
    description
  }
}
```

## 🔄 CI/CD Automatizado con GitHub Actions

Este proyecto utiliza **GitHub Actions** para gestionar un flujo de integración y despliegue continuo (CI/CD). Cada vez que se realizan cambios en la rama `main`, se ejecutan los siguientes pasos automáticamente:

1. **Clonación del código fuente**: 
   El workflow utiliza `actions/checkout` para clonar el repositorio y acceder a todos los archivos necesarios para la construcción de la imagen Docker.

2. **Generación de versión semántica**: 
   Se utiliza la acción [`PaulHatch/semantic-version`](https://github.com/marketplace/actions/git-semantic-version) para calcular una nueva versión basada en los cambios realizados, aplicando los patrones definidos:
   - `major:` para cambios importantes.
   - `feat:` para nuevas características.
   - Incremento automático del número de versión.

3. **Construcción de la imagen Docker**:
   - Se crea una imagen Docker etiquetada con la nueva versión (`astarthean/docker-graphql:{version}`).
   - Se actualiza la etiqueta `latest` para reflejar siempre la última versión estable.

4. **Publicación de la imagen en Docker Hub**:
   La imagen construida se sube automáticamente al repositorio de Docker Hub ([astarthean/docker-graphql](https://hub.docker.com/r/astarthean/docker-graphql)).

### Workflow de GitHub Actions

Para que worflow funcione hay que configurar las variables secretas `DOCKER_USER` (nombre en docker) y `DOCKER_PASSWORD` (token generado por docker)

```yaml
name: Docker Image CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      with:
        fetch-depth: 0

    - name: Git Semantic Version
      uses: PaulHatch/semantic-version@v4.0.3
      with:
        major_pattern: "major:"
        minor_pattern: "feat:"
        format: "${major}.${minor}.${patch}-prerelease${increment}"
      id: version

    - name: Docker login
      env:
        DOCKER_USER: ${{ secrets.DOCKER_USER }}
        DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      run: |
        docker login -u $DOCKER_USER -p $DOCKER_PASSWORD

    - name: Build Docker Image
      env:
        NEW_VERSION: ${{ steps.version.outputs.version }}
      run: |
        docker build -t astarthean/docker-graphql:$NEW_VERSION .
        docker build -t astarthean/docker-graphql:latest .

    - name: Push Docker Image
      env:
        NEW_VERSION: ${{ steps.version.outputs.version }}
      run: |
        docker push astarthean/docker-graphql:$NEW_VERSION
        docker push astarthean/docker-graphql:latest
```
