# Proyecto Final - Chat con WebSockets

## Resumen

Para su proyecto final realizarán una app parecida en funcionalidad a la siguiente: [app](https://hopeful-morse-552701.netlify.app/#/)

Deberán de incluir un diseño diferente

**Deberan usar Redux para manejar el estado excepto a lo relacionado con el WebSocket**

## [Base URL](https://acapp.herokuapp.com/)

## Lista de Endpoints

#### POST

`/login` - Para pedir un nuevo token y poder acceder a los endpoints protegidos

**requestBody:**

```json
{
  "username": "test@test.com",
  "password": "test123"
}
```

**response:**

```json
{
  "access_token": "token",
  "refresh_token": "refres_token",
  "userId": "1"
}
```

`/users` - Para crear un nuevo usuario

**requestBody:**

```json
{
  "username": "test@test.com",
  "password": "test123",
  "name": "Carlos Reyes"
}
```

**response:**

```json
{
  "id": "1"
}
```

#### GET

`/ping` - Endpoint para verificar el status del servidor

**requestBody:**

**response:**

```json
{
  "message": "pong"
}
```

`/ws` - Endpoint para conectarse al protocol WebSocket

## Como conectarse al endpoint de WebSocket

```js
const [socket, setSocket] = useState(null)
const accessToken = useSelector(state => state.access_token)

useEffect(() => {
  if (accessToken) {
    const encoded = encodeURI(accessToken)
    setSocket(new WebSocket('ws://localhost:8080/ws', ['token', encoded]))
  }
}, [accessToken])
```

El `access_token` se obtiene de hacer login

## Como interactuar con la conexión WebSocket

#### Message

Esto es el cuerpo de un Message la entidad requerida para comunicarse con el servidor WS

```json
{
  "action": "join-room",
  "message": "test",
  "target": {
    "id": 1,
    "name": "general" // Target ocupa un Room
  },
  "sender": {
    "id": 1,
    "name": "Carlos Reyes" // Sender es un usuario o el sistema
  }
}
```

#### Action

| Type              | Description                                                                       |
| ----------------- | --------------------------------------------------------------------------------- |
| send-message      | Es la acción que usaras para comunicarte al servidor                              |
| join-room         | Es la accion que usaras para indicarle al servidor que te quieres unir a una sala |
| leave-room        | Para dejar una sala                                                               |
| user-join         | El servidor te indica que un usuario se unio                                      |
| user-left         | El servidor te indica que un usuario abandono el servidor                         |
| join-room-private | Es la acción que usaras para iniciar una conversación privada                     |

## Leer más

[WS MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
