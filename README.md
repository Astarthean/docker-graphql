<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[![Docker Image Version](https://img.shields.io/docker/v/astarthean/docker-graphql/latest)](https://hub.docker.com/r/astarthean/docker-graphql)
[![GitHub Issues](https://img.shields.io/github/issues/Astarthean/docker-graphql)](https://github.com/Astarthean/docker-graphql/issues)
[![License](https://img.shields.io/badge/license-UNLICENSED-blue.svg)](LICENSE)

<p align="center">
  API GraphQL construida con NestJS implementando Docker y CI/CD automatizado
</p>

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
