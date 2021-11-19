# Testing dbt project: `jaffle_shop`

`jaffle_shop` is a fictional ecommerce store. This dbt project transforms raw data from an app database into a customers and orders model ready for analytics.

This repo was forked from the [original project](https://github.com/dbt-labs/jaffle_shop)

## Why this repo?

I needed a quick and easy way to test that I had installed dbt correctly on my
machine. I wanted it to be usable for personal development without needing to
start a trial account with a cloud database.

### What is this repo?

What this repo _is_:

- A self-contained playground dbt project, useful for testing out scripts, and communicating some of the core dbt concepts.

What this repo _is not_:

- A tutorial — check out the [Getting Started Tutorial](https://docs.getdbt.com/tutorial/setting-up) for that. Notably, this repo contains some anti-patterns to make it self-contained, namely the use of seeds instead of sources.
- A demonstration of best practices — check out the [dbt Learn Demo](https://github.com/dbt-labs/dbt-learn-demo) repo instead. We want to keep this project as simple as possible. As such, we chose not to implement:
  - our standard file naming patterns (which make more sense on larger projects, rather than this five-model project)
  - a pull request flow
  - CI/CD integrations
- A demonstration of using dbt for a high-complex project, or a demo of advanced features (e.g. macros, packages, hooks, operations) — we're just trying to keep things simple here!

### What's in this repo?

This repo contains [seeds](https://docs.getdbt.com/docs/building-a-dbt-project/seeds) that includes some (fake) raw data from a fictional app.

The raw data consists of customers, orders, and payments, with the following entity-relationship diagram:

![Jaffle Shop ERD](/etc/jaffle_shop_erd.png)

## Using this project to test your setup

### Install dbt, clone the repo, create / update `profiles.yml`

I did this automatically with my setup scripts. You can find them [here](https://github.com/erika-e/dotfiles)

### Open the folder for this repository in VSCode

This should open up all the files in the repo. You'll need a terminal up for the
next step. The original instructions advised setting up a profile, you can follow
those if you prefer. This is an opinionated version of `jaffle_shop` that assumes
you want to run Postgres, locally, in Docker.

### Make the Postgres database in docker

First, run `docker pull postgres` in the terminal to pull the base image of
Postgres from the docker registry.

Next, make a database by running the following:

```zsh
docker run --rm --name jaffle-pg -e POSTGRES_PASSWORD=docker -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data postgres;
```

This is adapted from a hiring challenge created by [Kelly Burdine](https://github.com/KellyBurdine). Let's break down what the docker command is doing.

- `docker run` is the command we're running to start the container
- `--rm` as an option means to remove the container when it exits
- `--name` gives the container a name so docker doesn't call it something random
- `-e` is an option for setting environment variables which is used to set a password
- `-d` tells docker to run the container in the background
- `-p` exposes the ports, [here](https://www.ctl.io/developers/blog/post/docker-networking-rules) is a hopefully accurate explainer on what it's doing
- `-v` simplistically, this option persists the data outside the container to the path provided. Not simplistically [here's the docs](https://docs.docker.com/engine/reference/commandline/run/#mount-volume--v---read-only)
- `postgres` is the docker image to use, more on the official hub page [here](https://hub.docker.com/_/postgres)

I use [DBeaver community edition](https://dbeaver.io/download/) because it's free.
To connect to your now-running database, open up DBeaver. Click the plug looking
icon to create a new connectin, choose `Postgres` and set your `Main` connnection
properties to the following:

- `Host:` `localhost`
- `Port:` `5432`
- `Database:` `postgres`
- `Password:` `docker`

Click test connection and if everything looks good, click ok.

### Check your dbt setup

Run `dbt debug` which will print a nice report if you got everything right.

### Seed the raw data to your container

Run `dbt seed`. If you open up DBeaver and refresh your connection, you should
see three tables under the public schema: `raw_customers`, `raw_orders` and
`raw_payments`

### Run dbt

As of dbt 0.21.0 you can run `dbt build` to build and test the project.

### Create the documentation site

Run `dbt docs generate`, open a new terminal then run `dbt docs serve`

## What is a jaffle?

A jaffle is a toasted sandwich with crimped, sealed edges. Invented in Bondi in 1949, the humble jaffle is an Australian classic. The sealed edges allow jaffle-eaters to enjoy liquid fillings inside the sandwich, which reach temperatures close to the core of the earth during cooking. Often consumed at home after a night out, the most classic filling is tinned spaghetti, while my personal favourite is leftover beef stew with melted cheese.

---
For more information on dbt:

- Read the [introduction to dbt](https://docs.getdbt.com/docs/introduction)
- Read the [dbt viewpoint](https://docs.getdbt.com/docs/about/viewpoint)
- Join the [dbt community](http://community.getdbt.com/)

---
