# Daily Journal

A small server-rendered journaling web app built with Node.js, Express, EJS and MongoDB. You write entries on a compose page, they're stored in MongoDB, and the home page lists them with a link to read each one in full.

It's a compact example of a classic Express + EJS + Mongoose stack: a handful of routes, a single Mongoose model, shared header/footer partials and Bootstrap 3 for styling.

## Features

- **Journal feed** on the home page, listing every saved entry with a "Read more" button.
- **Compose page** (`/compose`) with a title and body form for writing new entries.
- **Entry detail page** showing the full text of a single post.
- **Persistent storage** in MongoDB via Mongoose.
- **Automatic seeding**: if the database is empty on first visit, three sample entries ("Day 1"–"Day 3") are inserted.
- Static **About** and **Contact** pages.

## Tech stack

| Layer      | Technology                         |
| ---------- | ---------------------------------- |
| Runtime    | Node.js                            |
| Web server | Express 4 (+ body-parser)          |
| Templates  | EJS 2                              |
| Database   | MongoDB with Mongoose 6            |
| Styling    | Bootstrap 3 (CDN) + custom CSS     |

## Prerequisites

- [Node.js](https://nodejs.org/) 12 or newer (required by Mongoose 6) and npm
- [MongoDB](https://www.mongodb.com/try/download/community) running locally on the default port `27017`

## Getting started

```bash
# 1. Clone the repository
git clone https://github.com/nkelava/daily-journal.git
cd daily-journal

# 2. Install dependencies
npm install

# 3. Make sure MongoDB is running locally (example for a native install)
mongod

# 4. Start the app
npm start
```

Then open <http://localhost:3000>. On the first request the app seeds the `journalDB` database with sample entries and redirects back to the home page.

### Development mode

`npm run dev` restarts the server on file changes using [nodemon](https://nodemon.io/). nodemon isn't listed in the project's dependencies, so install it first:

```bash
npm install -g nodemon   # or: npm install --save-dev nodemon
npm run dev
```

## Usage

| Route          | Method | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| `/`            | GET    | Home page with all journal entries (seeds DB if empty)   |
| `/compose`     | GET    | Form for writing a new entry                             |
| `/compose`     | POST   | Saves the entry (`postTitle`, `postContent`) and redirects home |
| `/post`        | POST   | Shows a single entry; expects `postId` in the form body  |
| `/about`       | GET    | About page                                               |
| `/contact`     | GET    | Contact page                                             |

Typical flow: open the home page, click **New entry**, fill in a title and text, click **Publish**, then use **Read more** on any entry to view it in full.

## Configuration

| Variable | Default | Description                       |
| -------- | ------- | --------------------------------- |
| `PORT`   | `3000`  | Port the HTTP server listens on.  |

```bash
PORT=8080 npm start
```

The MongoDB connection string is hardcoded in `app.js` as `mongodb://localhost:27017/journalDB`. To use a different database (for example a hosted MongoDB instance), edit the `mongoose.connect(...)` call in `app.js`.

## Project structure

```text
daily-journal/
├── app.js               # Express server: DB connection, Post model, seed data and all routes
├── package.json         # Dependencies and npm scripts (start, dev)
├── Procfile             # Process definition for Heroku-style hosting
├── public/
│   └── css/
│       └── styles.css   # Custom styles layered on top of Bootstrap
└── views/               # EJS templates
    ├── home.ejs         # Entry list
    ├── compose.ejs      # New entry form
    ├── post.ejs         # Single entry view
    ├── about.ejs        # About page
    ├── contact.ejs      # Contact page
    └── partials/
        ├── header.ejs   # <head>, Bootstrap link and navbar
        └── footer.ejs   # Page footer
```

## Known limitations

- The home and about/contact page copy is placeholder (lorem ipsum) text defined in `app.js`.
- There is no editing, deleting or authentication — any visitor can publish entries.
- The startup log always prints port 3000, even when `PORT` is set to something else.
- The `Procfile` currently reads `web: npm app.js`; for a working deploy it should be `web: npm start` (or `web: node app.js`), and the database URL would need to point at a reachable MongoDB instance.

## License

Released under the ISC license, as declared in `package.json`.
