# Sistema POS Lotería

## Overview

Sistema completo de punto de venta (POS) para tienda/lotería. Incluye gestión de ventas, vendedores, números, buscador rápido y carga de Excel.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Frontend**: React + Vite + Tailwind CSS + shadcn/ui
- **Charts**: Recharts
- **Forms**: react-hook-form + zod
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (ESM bundle)
- **File uploads**: multer + xlsx

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/          # Express API server (porta 8080, ruta /api)
│   └── pos-loteria/         # React frontend POS (ruta /)
├── lib/
│   ├── api-spec/            # OpenAPI spec + Orval codegen config
│   ├── api-client-react/    # Generated React Query hooks
│   ├── api-zod/             # Generated Zod schemas from OpenAPI
│   └── db/                  # Drizzle ORM schema + DB connection
│       └── src/schema/
│           ├── vendedores.ts
│           ├── numeros.ts
│           └── ventas.ts
```

## Database Schema

### vendedores
- id (serial PK)
- nombre (text)
- created_at (timestamp)

### numeros
- id (serial PK)
- numero (text, unique)
- vendedor_id (FK → vendedores)
- fecha (timestamp)

### ventas
- id (serial PK)
- numero (text)
- valor (numeric 12,2)
- fecha (timestamp)
- vendedor_id (FK → vendedores)

## API Endpoints

- `GET /api/vendedores` — listar vendedores
- `POST /api/vendedores` — crear vendedor
- `PUT /api/vendedores/:id` — actualizar vendedor
- `DELETE /api/vendedores/:id` — eliminar vendedor
- `GET /api/numeros` — listar números (paginado, filtrable)
- `GET /api/numeros/buscar?q=xxx` — búsqueda rápida
- `POST /api/numeros` — registrar número
- `GET /api/ventas` — listar ventas (paginado, filtrable por fecha/vendedor)
- `POST /api/ventas` — registrar venta
- `DELETE /api/ventas/:id` — eliminar venta
- `GET /api/dashboard/stats` — estadísticas del dashboard
- `GET /api/dashboard/ventas-por-vendedor` — rendimiento por vendedor
- `POST /api/excel/upload` — carga de datos desde Excel (.xlsx)

## Modules

1. **Dashboard** — Tarjetas de ventas del día/semana/mes, gráfico por vendedor
2. **Buscador** — Búsqueda rápida de números con debounce
3. **Ventas** — Lista paginada, filtros, registro y eliminación
4. **Números** — Lista y registro de números
5. **Vendedores** — CRUD completo con modal de edición
6. **Carga Excel** — Subida de archivo .xlsx para números o ventas

## Development

- API server: `pnpm --filter @workspace/api-server run dev`
- Frontend: `pnpm --filter @workspace/pos-loteria run dev`
- DB push: `pnpm --filter @workspace/db run push`
- Codegen: `pnpm --filter @workspace/api-spec run codegen`

## Excel Format

### Para números:
Columnas: `numero`, `vendedor`, `fecha` (opcional)

### Para ventas:
Columnas: `numero`, `valor`, `vendedor`, `fecha` (opcional)
