# API REST – Tienda de Cómics

Ejercicio de Programación Web 2026B (Unidad 2) – Universidad Santiago de Cali.

API REST construida con **Node.js + Express + Mongoose**, conectada a un store documental en **MongoDB Atlas** (base de datos `tienda_comics`, colecciones `autores` y `comics`). Permite **crear y consultar** cómics y autores.

## Modelo de datos

- **autores**: `nombre` (obligatorio), `nacionalidad`, `fechaNacimiento`.
- **comics**: `titulo` (obligatorio, único), `autor` (referencia `ObjectId` a `autores`), `editorial`, `anioPublicacion`, `precio` (obligatorio), `stock`.

Se usan **referencias** (no documentos incrustados) porque un autor puede tener muchos cómics.

## Estructura

```
├── data/                 # Datos de ejemplo (seed)
└── src/
    ├── app.js            # Servidor Express
    ├── seed.js           # Carga de datos de ejemplo
    ├── config/db.js      # Conexión a MongoDB Atlas
    ├── models/           # Esquemas Autor y Comic
    ├── controllers/      # Lógica de cada endpoint
    └── routes/           # Rutas /api/autores y /api/comics
```

## Cómo ejecutarlo

1. Instalar dependencias:
   ```bash
   npm install
   ```
2. Crear el archivo `.env` (ver `.env.example`) con la cadena de conexión de Atlas:
   ```env
   PORT=3000
   MONGODB_URI=mongodb+srv://USUARIO:CONTRASENA@cluster0.xxxxx.mongodb.net/tienda_comics
   ```
3. Cargar los datos de ejemplo (5 autores y 10 cómics):
   ```bash
   npm run seed
   ```
4. Levantar la API:
   ```bash
   npm run dev
   ```

## Endpoints

| Método | URL | Descripción |
|---|---|---|
| `GET` | `/` | Ruta de prueba |
| `POST` | `/api/autores` | Crear un autor |
| `GET` | `/api/autores` | Listar autores |
| `GET` | `/api/autores/:id` | Consultar un autor |
| `POST` | `/api/comics` | Crear un cómic |
| `GET` | `/api/comics` | Listar cómics (filtros `?editorial=` y `?autor=`) |
| `GET` | `/api/comics/:id` | Consultar un cómic |

### Ejemplos

```bash
# Crear un autor
curl -X POST http://localhost:3000/api/autores \
  -H "Content-Type: application/json" \
  -d '{"nombre":"Hayao Miyazaki","nacionalidad":"Japonesa","fechaNacimiento":"1941-01-05"}'

# Crear un cómic (usar el _id del autor)
curl -X POST http://localhost:3000/api/comics \
  -H "Content-Type: application/json" \
  -d '{"titulo":"Nausicaä del Valle del Viento","autor":"ID_DEL_AUTOR","editorial":"Tokuma Shoten","anioPublicacion":1982,"precio":68000,"stock":4}'

# Listar cómics de DC Comics
curl "http://localhost:3000/api/comics?editorial=DC%20Comics"
```
