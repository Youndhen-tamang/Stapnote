# Ecommerce

Three independent repos. Nothing is shared: each has its own `package.json`, lockfile, `.env`, and git history.

| Repo | Path | URL |
| --- | --- | --- |
| API | `backend/` | http://localhost:4000 |
| Storefront + agency admin | `storefront/` | http://{slug}.localhost:3000 |
| Super admin | `super-admin/` | http://localhost:3004/super-admin/dashboard |

Start each in its own terminal:

```bash
cd backend && npm install && npm run migrate && npm run dev
cd storefront && npm install && npm run dev
cd super-admin && npm install && npm run dev
```
