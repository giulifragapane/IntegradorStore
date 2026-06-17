# Foodstore — Store (cliente)

**Video de presentación:** [carpeta en Google Drive](https://drive.google.com/drive/folders/1VGVLJdY9Qo6D388iF_YlcTaDRHvbVLUC?usp=drive_link)

Tienda web pública del proyecto **Foodstore**. Permite a los clientes navegar el catálogo, armar un carrito, registrarse o iniciar sesión, gestionar direcciones de entrega, confirmar pedidos —con efectivo, transferencia o Mercado Pago— y seguir el estado de sus pedidos en tiempo real vía WebSocket.

Consume la API del backend en [`IntegradorBackend`](../IntegradorBackend). Para operar el catálogo y los pedidos desde el staff, existe el panel en [`IntegradorAdmin`](../IntegradorAdmin).

## Stack

| Categoría | Tecnología |
|-----------|------------|
| Framework UI | React 19 |
| Lenguaje | TypeScript |
| Bundler / dev server | Vite 8 |
| Routing | React Router DOM 7 |
| Datos del servidor | TanStack Query 5 |
| Formularios | TanStack Form |
| Estado de cliente | Zustand (carrito con persistencia en `localStorage`, sesión) |
| HTTP | Axios (`withCredentials` para cookie JWT) |
| Estilos | Tailwind CSS 4 |
| Linting | ESLint + TypeScript ESLint |

## Requisitos previos

- [Node.js](https://nodejs.org/) (LTS recomendado)
- [pnpm](https://pnpm.io/)
- Backend corriendo en `http://localhost:8000` (ver [`IntegradorBackend`](../IntegradorBackend))

## Cómo correr en local

1. Entrá a la carpeta del proyecto:

```bash
cd store_final
```

2. Instalá dependencias:

```bash
pnpm install
```

3. Configurá variables de entorno copiando `.env.example` a `.env` y ajustando los valores si hace falta:

```env
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000/api/v1/ws/pedidos
```

4. Levantá el servidor de desarrollo:

```bash
pnpm dev
```

5. Abrí en el navegador la URL que muestra la terminal (por defecto `http://localhost:5173`). Si el admin ya usa el puerto 5173, Vite asignará otro —por ejemplo `5174`—; usá siempre la URL impresa en consola.

### Otros comandos

```bash
pnpm build    # build de producción
pnpm preview  # previsualizar el build
pnpm lint     # ESLint
```

## Qué hay en el repositorio

Arquitectura por **features** (dominio de negocio) más código compartido en `shared/`:

```
src/
  router/          # rutas de la app (AppRouter)
  shared/          # cliente Axios, NavBar
  features/
    catalog/       # listado y detalle de productos
    cart/          # carrito (Zustand + localStorage)
    auth/          # registro, login, perfil
    checkout/      # direcciones y finalización de compra
    orders/        # pedidos del cliente, cancelación y tiempo real
```

**Módulos principales**

- **Catálogo:** productos públicos con detalle en `/products/:id`.
- **Carrito:** agregar, modificar cantidades y persistir entre recargas.
- **Auth:** registro e inicio de sesión de clientes (`CLIENT`).
- **Checkout:** selección de dirección, forma de pago y creación del pedido; redirección a Mercado Pago cuando corresponde.
- **Pedidos:** historial del cliente, cancelación y actualización en vivo con WebSocket.

Los imports usan el alias `@/` → `src/`.

## Rutas

| Ruta | Descripción |
|------|-------------|
| `/` | Listado de productos |
| `/products/:id` | Detalle de producto |
| `/cart` | Carrito |
| `/login` | Inicio de sesión |
| `/register` | Registro |
| `/checkout` | Finalizar compra |
| `/addresses` | Direcciones del cliente |
| `/orders` | Mis pedidos |
| `/profile` | Perfil |

## Documentación adicional

- [`DOCUMENTACION-STORE-APP.md`](./DOCUMENTACION-STORE-APP.md) — flujo funcional y detalle de módulos
- [`GUIA-TRANSICION-ARQUITECTURA.md`](./GUIA-TRANSICION-ARQUITECTURA.md) — mapa de la arquitectura por features
