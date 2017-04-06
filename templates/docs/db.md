# Database

Buffalo uses [github.com/markbates/pop](https://github.com/markbates/pop) as its default database package for migrations, transactions, basic ORM functionality, and more.

<%= title("Getting Started") %>

Buffalo supports [PostgreSQL](https://www.postgresql.org/) (default), [MySQL](https://www.mysql.com/), and [SQLite3](https://sqlite.org/). When you generate a new Buffalo application you can change this with the `--db-type` flag. It is also possible to skip generation of all database components with the `--skip-pop` flag.

<%= title("database.yml", {name: "configuring"}) %>

When you first generate a Buffalo application a `database.yml` file will be generated for you, based on the type of database that was selected with the `--db-type` flag, with PostgreSQL being the default.

<%= code("yaml", {file: "database.yml"}) { %>
development:
  dialect: postgres
  database: myapp_development
  user: postgres
  password: postgres
  host: 127.0.0.1
  pool: 5

test:
  url: {{envOr "TEST_DATABASE_URL" "postgres://postgres:postgres@127.0.0.1:5432/myapp_test"}}

production:
  url: {{envOr "DATABASE_URL" "postgres://postgres:postgres@127.0.0.1:5432/myapp_production"}}
<% } %>

**CONFIGURE THIS FILE!**

Make sure to set up the appropriate usernames, passwords, hosts, etc... that are appropriate for the environment that will be running the application. Buffalo **does not** create these databases, or start up any services for you.

In the generated `database.yml` file there is a template helper, `envOr`, that will attempt to find the the ENV var with that name, in this case `DATABASE_URL`, if that ENV does not exist, it will load the "default" string.

<%= title("Creating Databases") %>

Once the `database.yml` has been configured with the appropriate settings, and the database server is running, Buffalo can create all of the databases in the `database.yml` file with a simple command:

```text
$ buffalo db create -a
```

You can also create just one of the configured databases by using the `-e` flag and the name of the database:

```text
$ buffalo db create -e test
```

<%= title("Dropping Databases") %>

Buffalo can drop all of your databases, should you want to, with one command:

```text
$ buffalo db drop -a
```

You can also drop just one of the configured databases by using the `-e` flag and the name of the database:

```text
$ buffalo db drop -e test
```

<%= partial("docs/db/list.md") %>
<%= partial("docs/db/model.md") %>
<%= partial("docs/db/fizz.md") %>
<%= partial("docs/db/sql.md") %>

<%= title("Running Migrations") %>

Once migrations have been created they can be run with either of the following commands:

```text
$ buffalo db migrate
$ buffalo db migrate up
```

Both commands are identical, one is shorter to type!

More information about the migration command be found by running:

```text
$ buffalo db migrate --help

Runs migrations against your database.

Usage:
  buffalo db migrate [flags]
  buffalo db migrate [command]

Aliases:
  migrate, m


Available Commands:
  down        Apply all of the 'down' migrations.
  reset       The equivalent of running `migrate down` and then `migrate up`
  up          Apply all of the 'up' migrations.

Global Flags:
  -c, --config string   The configuration file you would like to use.
  -d, --debug           Use debug/verbose mode
  -e, --env string      The environment you want to run migrations against. Will use $GO_ENV if set. (default "development")
  -p, --path string     Path to the migrations folder (default "./migrations")

Use "buffalo db migrate [command] --help" for more information about a command.
```
