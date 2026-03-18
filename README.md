# Costos Lomas APP

Sistema de consolidacion y analisis de costos de construccion para **Lomas de San Angel**. Sincroniza datos desde Airtable hacia una base de datos PostgreSQL local y presenta dashboards analiticos interactivos.

## Tech Stack

- **Framework:** Next.js 16 (App Router) + React 19 + TypeScript 5.9
- **Base de datos:** PostgreSQL 16 (Docker) + Drizzle ORM
- **Visualizacion:** Recharts 3.8
- **Estilos:** Tailwind CSS v4
- **Sincronizacion:** Airtable API → PostgreSQL (scripts custom con TSX)

## Estructura del Proyecto

```
src/
├── app/
│   ├── layout.tsx                    # Sidebar + layout principal
│   ├── dashboard/
│   │   ├── comparativo-unidad-costo/ # Dashboard 1: Comparativo UC
│   │   └── costos-destino/           # Dashboard 2: Costos por Destino
│   ├── tabla/[tableKey]/             # Visor de tablas dinamico
│   └── api/
│       ├── dashboard/                # API endpoints para dashboards
│       ├── destinos-agrupados/       # API destinos agrupados
│       └── tabla/                    # API tablas genericas
├── db/
│   ├── index.ts                      # Pool de conexion PostgreSQL
│   └── schema.ts                     # Schema Drizzle ORM
└── sync/
    ├── index.ts                      # Entry point de sincronizacion
    ├── airtable-client.ts            # Cliente Airtable
    ├── config.ts                     # Configuracion de tablas
    ├── sync-table.ts                 # Logica de sync por tabla
    └── transformers/                 # Transformadores por tabla
```

## Dashboards

### 1. Comparativo Unidad de Costo
Comparacion de costos entre unidades de costo (etapas constructivas) a traves de multiples destinos y periodos.

### 2. Costos Detallados por Destino
Ficha analitica completa para un destino especifico con 8 secciones:
- **KPIs** — Costo total, MOD, MOI, materiales, maquinaria + materiales clave (cemento, arena, piedrin, hierro, block)
- **Desglose por UC** — Grafico de barras apiladas con toggle valores/porcentaje
- **Tabla Resumen UC** — Tabla ordenable con mini barras y rubro dominante
- **Actividades + MOD** — 4 sub-tabs (tabla completa, top 10 MOD, top 10 total, barras MOD)
- **Materiales Principales** — Top 30 materiales, grafico materiales clave, heatmap matrix materiales x UC
- **Participacion por Rubro** — Pie charts comparativo (destino global vs UC seleccionada)
- **Ranking UCs** — Barras horizontales con selector de metrica
- **Filtros avanzados** — Destino searchable, periodo, tipo/modelo, subdestino, UC multiple, rango de fechas

### Tablas
Visor dinamico para explorar las tablas de datos:
- Actividades y Costos
- Destinos
- Materiales
- Salida de Materiales
- Combustibles
- Despachos de Combustible
- Uso de Maquinaria

## Requisitos

- **Node.js** >= 20
- **Docker Desktop** (para PostgreSQL)
- **Airtable API Key** (para sincronizacion)

## Instalacion

```bash
# 1. Clonar repositorio
git clone https://github.com/rpjorge96/CostosLomas.git
cd CostosLomas

# 2. Instalar dependencias
npm install

# 3. Configurar variables de entorno
cp .env.example .env.local
# Editar .env.local con:
#   DATABASE_URL=postgresql://postgres:postgres@localhost:5432/costos_lomas
#   AIRTABLE_API_KEY=pat...
#   AIRTABLE_BASE_ID=app...

# 4. Iniciar PostgreSQL
docker compose up -d

# 5. Ejecutar migraciones
npm run db:push

# 6. Sincronizar datos desde Airtable
npm run sync

# 7. Iniciar servidor de desarrollo
npm run dev
```

La aplicacion estara disponible en [http://localhost:3000](http://localhost:3000).

## Scripts

| Comando | Descripcion |
|---------|-------------|
| `npm run dev` | Servidor de desarrollo (Next.js + Turbopack) |
| `npm run build` | Build de produccion |
| `npm run sync` | Sincronizar todas las tablas desde Airtable |
| `npm run sync:periods` | Sincronizar solo periodos |
| `npm run sync:tables` | Sincronizar solo tablas |
| `npm run db:push` | Aplicar schema a la base de datos |
| `npm run db:studio` | Abrir Drizzle Studio (port 4983) |
| `npm run db:generate` | Generar migraciones |

## Inicio Rapido (Dev)

```bash
# Iniciar DB + servidor en un solo comando
./start-dev.cmd
```

## Base de Datos

PostgreSQL 16 corre en Docker con la siguiente configuracion:

| Parametro | Valor |
|-----------|-------|
| Host | localhost |
| Puerto | 5432 |
| Usuario | postgres |
| Password | postgres |
| Database | costos_lomas |

## Licencia

Proyecto privado - Todos los derechos reservados.
