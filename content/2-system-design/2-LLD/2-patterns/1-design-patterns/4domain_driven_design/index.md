

- Organizes code around features/domains instead of layers.

```python
src/modules/user/
├── user.controller.ts
├── user.service.ts
├── user.repository.ts
├── user.schema.ts
├── user.types.ts
└── index.ts
```

- Production grade architecture.

```ts
src/
│── app.ts                 # App bootstrap (Express/Fastify/Nest)
│── server.ts              # Server start / DB init
│
├── config/                # Global configuration & DB setup
│   ├── env.ts             # Zod-validated env vars
│   ├── logger.ts          # Winston/Pino logger
│   ├── db/
│   │   ├── postgres.ts    # Prisma client init
│   │   ├── mongo.ts       # MongoDB client init
│   │   └── index.ts       # Factory to select DB at runtime
│
├── modules/               # Domain-based feature modules
│   ├── admin/
│   │   ├── admin.controller.ts
│   │   ├── admin.service.ts
│   │   ├── admin.repository.ts        # Could be interface + impl
│   │   ├── admin.schema.ts            # Zod validation
│   │   ├── admin.types.ts             # Interfaces / DTOs
│   │   └── index.ts                   # Exports module API
│   │
│   ├── user/
│   │   ├── user.controller.ts
│   │   ├── user.service.ts
│   │   ├── user.repository.ts
│   │   ├── user.schema.ts
│   │   ├── user.types.ts
│   │   └── index.ts
│   │
│   └── order/
│       ├── order.controller.ts
│       ├── order.service.ts
│       ├── order.repository.ts
│       ├── order.schema.ts
│       ├── order.types.ts
│       └── index.ts
│
├── repositories/          # DB-agnostic interfaces & factories
│   ├── interfaces/        # e.g., IUserRepository.ts
│   ├── postgres/          # Postgres implementations
│   │   ├── admin.repo.pg.ts
│   │   ├── user.repo.pg.ts
│   │   └── order.repo.pg.ts
│   ├── mongo/             # Mongo implementations
│   │   ├── admin.repo.mongo.ts
│   │   ├── user.repo.mongo.ts
│   │   └── order.repo.mongo.ts
│   └── index.ts           # Factory to select correct repo per DB
│
├── middlewares/           # Global middlewares
│   ├── validate.ts        # Zod request validator
│   ├── auth.ts
│   └── errorHandler.ts
│
├── utils/                 # Reusable helpers
│   ├── hash.ts
│   ├── jwt.ts
│   ├── date.ts
│   └── logger.ts
│
├── events/                # Event-driven architecture (Kafka, Redis, etc.)
│   ├── producer.ts
│   ├── consumer.ts
│   └── handlers/
│       ├── userCreated.handler.ts
│       └── orderCreated.handler.ts
│
├── routes/                # Route registration
│   ├── admin.routes.ts
│   ├── user.routes.ts
│   ├── order.routes.ts
│   └── index.ts
│
└── types/                 # Shared / global types
    ├── common.ts           # Pagination, APIResponse<T>, etc.
    └── db.ts               # DB client types (Postgres/Mongo)

```