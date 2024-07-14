---
layout: post
title: How to setup multiple admin accounts on Immich
---

This guide assumes you're running Immich using the [recommended](https://immich.app/docs/install/docker-compose) installation method.

## connect to the postgress database docker container

`docker exec -it immich_postgres bash`

## connect to the postgress shell in the container and load the immich database

`psql immich postgres`

## check which users are currently admin users

```psql
immich=# select email,"isAdmin" from users;
```

## promote an existing user to the admin role

```psql
UPDATE users SET "isAdmin"='t' WHERE email='email@example.com';
```

And that's it!
