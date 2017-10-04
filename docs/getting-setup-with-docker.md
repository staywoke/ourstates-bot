**[↤ Developer Overview](../README.md)**

Getting Setup with Docker ( Recommended )
===

![Docker Logo](img/docker-logo.png "Docker Logo")

#### Good News, You only need to do this initial setup once ;)

Requirements
---

* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/install/)


Build Docker Containers
---

Build the Docker Containers we need:

```bash
cd ./docker
docker-compose up --build -d nginx redis
```

Install Dependencies
---

Connect to Docker Container to prepare to run Bash Commands:

```bash
cd ./docker
docker-compose exec ourstatesbot bash
```

First, let's remove folders that may cause problems:


```bash
rm -fr node_modules
rm -fr vendor
```

Now we can install our dependencies:

```bash
yarn install
composer install
php artisan key:generate
```

Build Chat Bot
---

Now that we have all the dependencies installed, we can build the Chat Bot.

#### Build for Development

```bash
yarn run dev
```

#### Build for Production

```bash
yarn run production
```


Accessing the Chat Bot
---

Now you can open your web browser to [http://localhost](http://localhost)

Internally we are using [http://ourstates-bot.loc](http://ourstates-bot.loc) as a developer domain.  This can be added to your `/etc/hosts` by adding:

```
127.0.0.1 ourstates-bot.loc
```

Managing Docker
---

From your local development machine, you can manage our docker containers using `docker-compose`

```bash
cd ./docker
```

| command                  | description                                                     |
|--------------------------|-----------------------------------------------------------------|
| `docker-compose start`   | Start Docker Services                                           |
| `docker-compose logs`    | View output from Docker containers                              |
| `docker-compose stop`    | Stop Docker Services                                            |
| `docker-compose restart` | Restart Docker Services                                         |
| `docker-compose down`    | Stop and remove Docker Containers, Networks, Images & Volumes   |


#### Flush Docker Redis Cache

```bash
cd ./docker
docker-compose run redis redis-cli -h redis FLUSHALL
```
