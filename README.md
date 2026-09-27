# Budget and spending tracker

A budget for one person, kept honestly a month at a time. Spending
lands in categories, every figure on the screen is the sum of the
entries behind it, and a month actually ends: it closes, it stops
moving, and what is left over or overspent follows the person into the
next one by a rule they can read.

## What finished looks like

A stranger can open the URL, register, prove the address is theirs, set
a budget for a month that starts on the day of the month they choose,
record what they spend against a category, close a month and carry what
is left into the next one, and be warned while there is still time to
act. On the reset day the whole thing turns over with nobody pressing
anything.

Finished does not mean running on a laptop. It means somebody who has
never met the person who built it can use it on the internet, and that
their month still turns over on the day when that laptop is shut.

## The road map

Eight sprints. Each one makes a different part of that sentence true,
and the order is not arbitrary: take any sprint out and the sentence
stops being true.

| # | Sprint | What it makes possible |
|---|---|---|
| 1 | Getting in | Somebody can make an account and come back to it |
| 2 | What went out | Spending is recorded by category, and totals are the sum of it |
| 3 | The shape of a month | A budget for a period that starts on the day they chose |
| 4 | Closing the books | A month ends, stops moving, and hands something to the next one |
| 5 | Told in time | Warned while there is still month left to act on |
| 6 | The day it turns over | The month closes and the warnings go out on their own |
| 7 | An address that is theirs | Mail goes to the person who typed the address |
| 8 | Out in the world | It is on the internet, at its own address, and it keeps time |

## The hard part

The edge of a month. Everything difficult in this product happens
there: a receipt dated the 31st typed in on the 2nd, a budget raised on
the 20th when most of it is already spent, money that is committed but
not yet gone, a category merged into another after six months of
history, and the question nobody answers until they have to, which is
whether an overspent month follows you.

Rollover is a policy with more than one right answer. This road map
does not pick one for you. It makes you pick, write the choice where a
user can read it, and then live with it in every sprint afterwards.

Whatever you choose, one thing is not negotiable: a month's figures are
derived from the transactions inside it. A running total somebody
decrements cannot answer "where did that number come from", and that is
the only question anybody asks a budget.

## Working on it

The sprints, their briefs and the tickets under them are in Blacksmith.
Start with sprint one; each sprint opens as the one before it closes.

The reading attached to a sprint is worth opening before its first
ticket rather than after. It carries the parts a ticket deliberately
does not: what the alternatives were, and which one you are choosing
between.

## What this is built with

The whole product: an API and the screens that use it.

- **Django** with **Django REST Framework** for the API.
- **SimpleJWT** for authentication, against a custom user model in `apps/users`.
- **drf-spectacular** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── config/
│   ├── settings/        # base, development, production
│   └── urls.py          # where routes are mounted
├── apps/
│   └── users/           # the custom user model, and auth
├── utils/               # shared helpers, base model
├── manage.py
└── requirements.txt

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a serializer or a route. |
| `blacksmith make:resource Post` | Scaffold a model, serializer, viewset and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run a Django management command, e.g. `blacksmith backend createsuperuser`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Django and React project. Nothing here is a dependency on us. |
