# Attendance Journal

School attendance journal: a table of students by lesson where a click on a cell marks or clears an absence. The interface is in Ukrainian. Built in January 2025 as a take-home assignment.

## Features

- Loads students, lesson columns and absences from the assignment's REST API and shows them as one table.
- A click on a cell marks the student absent with "H" or clears the mark. The table updates at once and rolls back if the request fails.
- The row number and the student's name open a page with the last name, first name and patronymic.
- Loading indicators, error cards with a retry button, and a toast message for every failed request (network error, 404, 500 or another status).
- An error boundary with a retry button, and a not found page for unknown addresses.

## Tech stack

- **Framework:** React 18, TypeScript 5
- **Data:** React Query 3, Axios 1
- **Routing:** React Router 7
- **UI:** Material UI 6 with Emotion, React Toastify 11, react-error-boundary 5, Roboto from Fontsource
- **Tooling:** Vite 6, ESLint 9 with typescript-eslint and simple-import-sort, Prettier 3
- **Hosting:** Vercel

## Getting started

You need Node.js 20 or later and access to the assignment's REST API.

```bash
git clone https://github.com/androfficial/react-attendance-journal.git
cd react-attendance-journal
npm install
npm run dev
```

Before `npm run dev`, create a `.env` file in the project root with these variables:

| Variable | Purpose |
| --- | --- |
| `VITE_API_URL` | Base URL of the assignment's REST API |
| `VITE_CLASS_KEY` | Class key; every request goes to `<VITE_API_URL>/<VITE_CLASS_KEY>` |

The dev server runs at http://localhost:5173.

## Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Starts the Vite dev server |
| `npm run build` | Type-checks with `tsc -b` and builds to `dist/` |
| `npm run preview` | Serves the production build locally |
| `npm run lint` | Runs ESLint |
| `npm run format` | Formats the TypeScript files in `src` with Prettier |
| `npm run format:check` | Checks the formatting without changing files |

## Project structure

```text
src/
  api/          Axios instance with error toasts, requests for students, columns and absences
  components/   students table, student details, loading, error and not found views, error boundary
  configs/      React Query client and toast container settings
  layouts/      centered layout for the details and not found pages
  services/     localStorage wrapper
  styles/       CSS variables, reset and Roboto font imports
  themes/       Material UI theme for the table
  types/        API models
```

## Notes

- Absence changes are React Query mutations with optimistic updates: `onMutate` writes the new mark to the cache, `onError` restores the previous data and `onSettled` refetches the absences.
- One Axios instance builds every URL from the base URL and the class key and shows a toast for each failed response, so the components only render loading and error states.
- `vercel.json` rewrites `/api/*` to the assignment's server, which is served over plain HTTP, so on Vercel the app reaches it from its own HTTPS origin.
- The assignment's API server is no longer online, so there is no live demo. The app works against any server that implements the same students, columns and absences endpoints.
