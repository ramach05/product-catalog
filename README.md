# Product Catalog

🇬🇧 **English** | [🇷🇺 Русский](README.ru.md)

[![CI](https://github.com/ramach05/product-catalog/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/ramach05/product-catalog/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An authenticated product catalog: login against a real API, a searchable and
sortable products table with pagination, selection across pages and an
add-product modal. React 18, TypeScript, Vite, Zustand, CSS Modules.

## Screens

| Login | Validation | Catalog | Add product |
| --- | --- | --- | --- |
| <img src="screenshots/login.png" alt="Login form" width="220" /> | <img src="screenshots/login-validation.png" alt="Login form with validation errors" width="220" /> | <img src="screenshots/main-page.png" alt="Products table with search and pagination" width="220" /> | <img src="screenshots/modal.png" alt="Add product modal" width="220" /> |

## Highlights

- **Session strategy follows user intent** — "remember me" stores the token in
  `localStorage` (survives browser restart), otherwise `sessionStorage` (dies
  with the tab); the token is re-validated via `GET /auth/me` on app start and
  a stale token forces logout.
- **Server-side search, sort and pagination** — debounced search, `limit`/`skip`
  pagination, column sorting (`sortBy`, `order`) all go through the API; page
  number and search query persist in `sessionStorage` (Zustand `persist`) and
  are restored on return.
- **Selection across pages** — row checkboxes plus a header "select all" that
  applies to the current page only while selections on other pages are kept.
- **Loading states everywhere** — progress bar on every fetch, a 5-row skeleton
  table before the first successful load, dismissible error message.
- **Add-product modal** — field validation, closes on backdrop click, "×" or
  `Escape`; new items go to local state with a success toast (no API write).

## Features

- Login form: required-field validation, per-field errors plus a form-level
  block (also for API errors). Demo credentials: login `emilys`, password
  `emilyspass`.
- Table columns: name (thumbnail + category), vendor, SKU, rating, price,
  actions; ratings below 3 are highlighted in red.
- Sort by name, vendor, rating, price — click a header to cycle asc → desc;
  sort state lives in the store for the session.
- "Refresh" resets search and page and reloads; "Logout" clears the token and
  persisted page/search and redirects to login.
- Protected route: unauthenticated visits to the catalog redirect to login.

## API

- **Products**: [DummyJSON Products](https://dummyjson.com/docs/products) —
  list, `/products/search`, sorting via `sortBy`/`order`, pagination via
  `limit`/`skip`.
- **Auth**: [DummyJSON Auth](https://dummyjson.com/docs/auth) — `POST
  /auth/login`, token check via `GET /auth/me` with `Authorization: Bearer
  <token>`.

In dev, requests are proxied by Vite: [vite.config.ts](vite.config.ts) maps
`/api/dummyjson` to `https://dummyjson.com`.

## Structure

```
├── src/
│   ├── api/         # API_BASE, authApi (login/getMe), productsApi (fetch/search)
│   ├── store/       # authStore (token, init, login, logout),
│   │                # productsStore (list, pagination, search, sort, selection, persist)
│   ├── pages/       # LoginPage, ProductsPage (+ CSS Modules)
│   ├── components/  # ProtectedRoute, Button, Input, Checkbox, ProductsTable,
│   │                # AddProductModal, Toast, ProgressBar, icons/
│   ├── hooks/       # useDebounce
│   ├── types/       # auth, product, component props
│   └── utils/       # storage (token local/session), validation, format, dom id
└── vite.config.ts   # dev proxy to dummyjson.com
```

## Implementation notes

- **Semantics and a11y**: `main`, `section`, `nav`, `header`, `footer`,
  skip-link, `visually-hidden`, ARIA attributes.
- **Styles**: CSS Modules with design tokens in `:root` ([index.css](src/index.css));
  shared primitives (Button, Input, Checkbox) reused across pages.
- **State**: Zustand only — no prop drilling for auth or table state; `persist`
  middleware scoped to page/search only.
- **Types**: strict TypeScript, shared types in [src/types/](src/types/).

## Quick start

Node.js 20+.

```bash
npm install
npm run dev    # http://localhost:5173
```

| Script | Description |
| --- | --- |
| `npm run dev` | dev server with hot reload |
| `npm run build` | typecheck + production build |
| `npm run lint` | ESLint |
| `npm run preview` | serve the production build |

## Stack

**React 18** · **TypeScript** · **Vite** · **React Router** · **Zustand** · **CSS Modules**

## License

MIT
