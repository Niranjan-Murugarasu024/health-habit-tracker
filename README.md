# Health Habit Tracker

A Next.js application for managing health habits and therapy sessions with authenticated user accounts.

## Getting Started

Install dependencies and start the development server:

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

Configure the environment variables required by the application before running it. Database access uses Turso/libSQL; see `drizzle.config.ts` for the expected connection settings.

## Weekly Habit Completion Analytics

The dashboard displays the authenticated user's habit completions for the last seven UTC calendar days. A completion is stored per habit and date, so repeating a completion request for the same habit on the same day does not create another record. Removing a habit cascades to its completion history.

The `/api/habit-completions` endpoint provides:

- `GET` — daily completion totals for the signed-in user.
- `POST` — record or remove today's completion with a JSON body such as:

  ```json
  {
    "habitId": 123,
    "completed": true
  }
  ```

Requests require the signed-in user's bearer token in the `Authorization` header. The API checks that the selected habit belongs to that user. It records completion history and does not update the existing habit `progress` or `streak` fields.

The table is defined in `src/db/schema.ts`, with SQL in `drizzle/0002_habit_completions.sql`. The SQL file has **not** been applied to the Turso database. Before enabling the API against a database, verify its existing schema and resolve the migration journal inconsistency; do not blindly run the migration against a database that may already contain the table.

## Useful Commands

```bash
npm run dev
npm run build
npx tsc --noEmit
```

## Deployment

This application can be deployed with [Vercel](https://vercel.com/). Configure the required environment variables in the deployment environment and ensure the Turso schema is prepared before enabling completion analytics.
