# Funcion Serverless con Azure Functions

Funcion serverless desarrollada en C# sobre el modelo isolated worker de Azure Functions, que expone un endpoint HTTP cuyo proposito es replicar, bajo el paradigma de computo bajo demanda, la misma funcionalidad de un microservicio tradicional. La funcion responde a una peticion GET devolviendo un mensaje personalizado en formato JSON.

## Descripcion

La aplicacion expone el endpoint `GET /api/hello`, el cual recibe un parametro `msj`, lo procesa y responde con un objeto JSON personalizado. A diferencia de un microservicio de larga ejecucion, la funcion solo se activa ante la llegada de una peticion y no mantiene un proceso permanente en memoria.

Ejemplo de uso:

```
GET /api/hello?msj=Ana
```

Respuesta:

```json
{
  "message": "Hola Ana desde Azure Functions"
}
```

Si no se envia el parametro `msj`, la funcion responde con un valor por defecto:

```json
{
  "message": "Hola Mundo desde Azure Functions"
}
```

## Tecnologias utilizadas

- Azure Functions (.NET 8 isolated worker)
- Azure Functions Core Tools v4
- Azurite (emulador local de Azure Storage)
- HTTP Trigger

## Estructura del proyecto

```
hello-function/
└── MyFunctionProj/
    ├── HelloFunction.cs
    ├── Program.cs
    ├── host.json
    ├── local.settings.json
    └── MyFunctionProj.csproj
```

## Requisitos previos

- .NET 8 SDK
- Azure Functions Core Tools v4
- Azurite (emulador de almacenamiento)

Instalacion de las herramientas mediante npm:

```bash
npm install -g azure-functions-core-tools@4 --unsafe-perm true
npm install -g azurite
```

## Configuracion

El archivo `local.settings.json` define las variables de entorno para la ejecucion local:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
}
```

## Ejecucion local

Primero, iniciar el emulador de almacenamiento Azurite en una terminal independiente:

```bash
azurite
```

Luego, en otra terminal y desde la carpeta del proyecto, levantar la funcion:

```bash
func start
```

Una vez iniciada, la consola mostrara el endpoint disponible:

```
hello: [GET] http://localhost:7071/api/hello
```

## Probar el endpoint

Desde el navegador:

```
http://localhost:7071/api/hello?msj=Ana
```

O desde la terminal:

```bash
curl "http://localhost:7071/api/hello?msj=Ana"
```

## Acceso a los servicios

| Servicio              | URL                                       |
|-----------------------|-------------------------------------------|
| Endpoint de la funcion| http://localhost:7071/api/hello?msj=Ana   |
| Pagina del host       | http://localhost:7071                     |
