# Servd — AI Recipe Platform

Servd helps you decide what to cook with the ingredients you already have. Keep a digital pantry, use AI to identify ingredients from a photo, and get recipe ideas based on your pantry or a dish you want to make. You can also browse recipes, save favorites, and keep them in a personal cookbook.

## What you can do

- **Manage a pantry:** add ingredients manually or scan a pantry/fridge photo; review, edit, and remove items.
- **Get AI cooking ideas:** ask for recipe suggestions based on pantry ingredients, or generate a detailed recipe by name.
- **Browse recipes:** explore recipes by category and cuisine, including recipes from TheMealDB.
- **Build a cookbook:** save recipes to your account, view your collection, and export a recipe as a PDF.
- **Use an authenticated account:** sign in with Clerk; user profiles, pantry items, recipes, and saved recipes are managed through Strapi.
- **Apply usage limits:** Arcjet protects AI-powered actions with limits for free and Pro tiers.

AI-generated recipes and ingredient recognition can be imperfect; review ingredients and instructions before cooking.

## Technology

- **Frontend:** Next.js 16 App Router, React 19, Tailwind CSS 4
- **Backend/API:** Strapi 5 with SQLite for local development; PostgreSQL can be configured
- **Authentication:** Clerk
- **AI:** Google Gemini for recipe generation and pantry-image recognition
- **Rate limiting/security:** Arcjet
- **Recipe discovery and images:** TheMealDB and, optionally, Unsplash

## Repository layout

```text
.
├── frontend/   # Next.js user interface and server actions
└── backend/    # Strapi API, content types, and admin panel
```

The frontend runs on `http://localhost:3000`. Strapi runs on `http://localhost:1337`, with its admin panel at `http://localhost:1337/admin`.

## Requirements

- Node.js 20–24
- npm
- API credentials for Clerk, Google Gemini, and Arcjet

## Run locally

### 1. Configure and start the backend

In a terminal:

```bash
cd backend
cp .env.example .env
```

Replace the placeholder secret values in `backend/.env` with unique values for your local environment, then install dependencies and start Strapi:

```bash
npm install
npm run develop
```

On the first run, open the Strapi admin URL and create an administrator account. Create an API token with the access needed by the frontend, then put it in `STRAPI_API_TOKEN` in the frontend environment file. Local development uses SQLite by default and stores its database under `backend/.tmp/`.

### 2. Configure and start the frontend

In a second terminal, create `frontend/.env.local` and add your own credentials:

```dotenv
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
GEMINI_API_KEY=
ARCJET_KEY=
NEXT_PUBLIC_STRAPI_URL=http://localhost:1337
STRAPI_API_TOKEN=
# Optional: recipe images from Unsplash
UNSPLASH_ACCESS_KEY=
```

Then install dependencies and start Next.js:

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:3000`. The frontend needs the backend running and the environment variables configured for authentication, AI features, and Strapi data access.

## Environment variables

| Variable | App | Purpose |
| --- | --- | --- |
| `APP_KEYS` | Backend | Strapi application keys |
| `API_TOKEN_SALT` | Backend | Salt for Strapi API tokens |
| `ADMIN_JWT_SECRET` | Backend | Strapi admin authentication |
| `TRANSFER_TOKEN_SALT` | Backend | Salt for Strapi transfer tokens |
| `JWT_SECRET` | Backend | Users & Permissions JWT signing |
| `ENCRYPTION_KEY` | Backend | Strapi secrets encryption |
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Frontend | Clerk publishable key |
| `CLERK_SECRET_KEY` | Frontend | Clerk server-side authentication |
| `GEMINI_API_KEY` | Frontend | Gemini recipe generation and image recognition |
| `ARCJET_KEY` | Frontend | Arcjet rate limiting |
| `NEXT_PUBLIC_STRAPI_URL` | Frontend | Base URL of the Strapi API |
| `STRAPI_API_TOKEN` | Frontend | Server-side access to Strapi |
| `UNSPLASH_ACCESS_KEY` | Frontend | Optional recipe image lookup |

For PostgreSQL, configure the Strapi database variables such as `DATABASE_CLIENT=postgres`, `DATABASE_HOST`, `DATABASE_PORT`, `DATABASE_NAME`, `DATABASE_USERNAME`, and `DATABASE_PASSWORD` in `backend/.env`. SQLite is the default for local development.

`NEXT_PUBLIC_FORCE_PRO` is a development-only testing switch that marks users as Pro. Leave it unset or set it to `false` for normal use; **do not enable it in a deployed environment**, since it bypasses the normal plan check.

## Available scripts

Run these from the relevant project directory:

| Directory | Command | Purpose |
| --- | --- | --- |
| `frontend/` | `npm run dev` | Start the Next.js development server |
| `frontend/` | `npm run build` | Create a production build |
| `frontend/` | `npm run start` | Serve a production build |
| `frontend/` | `npm run lint` | Run ESLint |
| `backend/` | `npm run develop` | Start Strapi in development mode |
| `backend/` | `npm run build` | Build the Strapi admin panel |
| `backend/` | `npm run start` | Start Strapi |

## Security and privacy

- Never commit `.env`, `.env.local`, API tokens, or generated database files. The backend and frontend `.gitignore` files exclude local environment files.
- Use your own credentials and unique Strapi secrets; do not reuse example or development values in production.
- The `NEXT_PUBLIC_` prefix means a value is exposed to the browser. Only use it for values intended to be public, such as the Clerk publishable key and Strapi URL.
- Pantry photos are sent to the configured Gemini service for ingredient recognition. Avoid uploading images containing information you do not want processed by that service.
- Protect your Strapi admin account and API token, and configure the required API permissions before exposing the backend publicly.

---

Made with 💗 by Pranav.
