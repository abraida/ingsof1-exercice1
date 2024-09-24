# API

## Login

**Descripción**: Permite que los usuarios inicien sesión.

### Request:

- Headers: Authorization: Basic {base64(email:password)}
- **POST** /api/v1/users/token

### Response:

- **Código HTTP:** 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
"refresh_token": "m9obiBEb2UiLCJpYXQiOjE1MTYyMzkwMj...",
    
    “role”: “user” //admin
    }
    
- **Código HTTP:** 401 Unauthorized si las credenciales son incorrectas.

## Register

**Descripción**: Permite que los usuarios se registren.

### Request:

- Headers:Content-Type:application/json
- **POST** /api/v1/users/register
- Body
    
    {
    "email": "abraida@fi.uba.ar",
    "nombre": "Agustin",
    "apellido": "Braida",
    "fecha_nacimiento": "23/02/2001",
    "genero": "masculino",
    "password": "password123",
    "avatar": "image_url"
    }
    

### Response:

- **Código HTTP:** 201 OK
    
    {
    
    "message": "Usuario registrado correctamente",
    
    }
    
- **Código HTTP:** 400 Bad Request

## Peliculas Recomendadas

**Descripción**: Permite que los usuarios no logueados requieran peliculas.

### Request:

- Headers: Authorization: Basic {base64(email:password)}
- **GET** api/v1/movies
- Query parameters: Title, Category, Actor
    
    Ejemplo: api/v1/movies?title=x&actor=a&actor=b&category=z
    

### Response:

- **Código HTTP:** 200 OK
{
"metadata" : {
"page": 5,
"per_page": 20,
"page_count": 20,
"total_count": 521,
}
"movies" : {....}
    
    }
    

## Calificar Películas

Descripción: Permite calificar películas a usuarios logueados con una puntuación del 1 al 10.

### Request

- Headers:  Authorization: Bearer {access_token}
- PUT /api/v1/ratings/{movie_id}
- Body:
    
    {
    "rating": 8
    }
    

### Response:

- **Código HTTP:** 200 OK
    
    {
      "message": "Calificación registrada exitosamente."
    }
    
- **Código HTTP:**  400 Bad Request: Si el puntaje no es válido.
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:**  404 Not Found: Si no existe pelicula con ese id

## Buscar Usuarios

Descripción: Permite a los usuarios buscar otros usuarios.

### Request

- Headers:  Authorization: Bearer {access_token}
- GET /api/v1/users?name=name

### Response:

- **Código HTTP:** 200 OK
    
    {
    "users" : {....}
    
    }
    
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado.

## Ver usuario

Descripción: Permite a los usuarios ver el perfil de otro usuario.

### Request

- Headers:  Authorization: Bearer {access_token}
- GET /api/v1/users/{user_id}

### Response:

- **Código HTTP:** 200 OK
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:** 404 Not Found

## Ver usuario reviews

Descripción: Permite a los usuarios ver las reviews de otro usuario.

### Request

- Headers:  Authorization: Bearer {access_token}
- GET /api/v1/users/{user_id}/reviews

### Response:

- **Código HTTP:** 200 OK
    
    {
    "reviews" : {....}
    
    }
    
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:**  404 Not Found

## Seguir usuario

Descripción: Permite a los usuarios seguir a otro usuario.

### Request

- Headers:  Authorization: Bearer {access_token}
- POST /api/v1/users/{user_id}/followers

### Response:

- **Código HTTP:** 200 OK
    
    {
      "message": "Solicitud de seguimiento enviada."
    }
    
- **Código HTTP:** 401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:** 404 Not Found

## Aceptar o Rechazar Solicitudes de Seguimiento

Descripción: Permite aceptar o rechazar solicitudes de seguimiento.

### Request

- Headers:  Authorization: Bearer {access_token}
- POST /api/v1/followers/{user_id}
- Body:
    
    {
    "action": “accept”, //or reject
    }
    

### Response

- **Código HTTP:** 200 OK
    
    {
      "message": "Solicitud aceptada."
    }
    
- **Código HTTP:** 401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:** 404 Not Found

## Ver seguidores

Descripción: Permite a los usuarios ver sus seguidores

### Request

- Headers:  Authorization: Bearer {access_token}
- GET /api/v1/followers

### Response:

- **Código HTTP:** 200 OK
    
    {
    "followers" : {....}
    
    }
    
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado.
- **Código HTTP:**  404 Not Found

## Editar Perfil

Descripción: editar el perfil del usuario.

### Request

- Headers:  Authorization: Bearer {access_token}
- PATCH /api/v1/users/{user_id}
- Body:
    
    {
    
    nuevo_nombre: nombre,
    
    …
    
    }
    

### Response:

- **Código HTTP:** 200 OK
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado o no es el usuario user_id.
- **Código HTTP:**  404 Not Found

## Borrar Perfil (Logueado)

Descripción: borrar el perfil del usuario.

### Request

- Headers:  Authorization: Bearer {access_token}
- DELETE /api/v1/users/{user_id}

### Response:

- **Código HTTP:** 200 OK
- **Código HTTP:**  401 Unauthorized: Si el usuario no está autenticado o no es el usuario user_id.
- **Código HTTP:**  404 Not Found

## Funciones de ADMIN

### Crear/Editar/Eliminar Usuarios ADMIN:

- POST /api/v1/admin/users
- PATCH /api/v1/admin/users/{user_id}
- DELETE /api/v1/admin/users/{user_id}

### Crear/Editar/Eliminar Categorías/Actores/Películas:

- POST /api/v1/admin/categories
- PATCH /api/v1/admin/categories/{category_id}
- DELETE /api/v1/admin/categories/{category_id}
- POST /api/v1/admin/actors
- PATCH /api/v1/admin/actors/{actor_id}
- DELETE /api/v1/admin/actors/{actor_id}
- POST /api/v1/admin/movies
- PATCH /api/v1/admin/movies/{movie_id}
    
    Body:
    
    {
    "categories": [ …],
    "actors": [… ],
    }
    
- DELETE /api/v1/admin/movies/{movie_id}

# Modelo de negocio

Se muestra un diagrama de clases simplificado como representacion preliminar del modelo de negocios:

![](https://imgur.com/1TK0uFi.png)

# Arquitectura de software

La arquitectura propuesta se diseño alrededor de una arquitectura cliente servidor y una "layered architecture" o arquitectura de capas, siendo estas capas: El cliente y el server, a su ves estructurado con las capas controllers, services, domain y persistency. Para mayor completitud se muestra la arquitectura segun el modelo de vistas 4+1.

## Vista logica

![](https://imgur.com/kn6S5pr.png)  

## Vista de procesos

Se muestra como ejemplo unicamente el proceso de dejar un rating

![](https://imgur.com/W4YmIAi.png)  

## Vista de Desarrollo

![](https://imgur.com/4OYQplU.png)  


## Vista Fisica

![](https://imgur.com/35j7LNi.png)


## Vista de escenarios

![](https://imgur.com/JoOYu6g.png)
