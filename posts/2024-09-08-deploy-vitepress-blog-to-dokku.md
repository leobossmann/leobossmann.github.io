---
layout: doc
title: Deploying Vitepress Blog to Dokku
date: 2024-09-08
tags: 
- Vitepress
- Dokku
- Deployment
---

<Title/>

I have recently set up a [Dokku](https://dokku.com) instance on a simple VPS, naturally I wanted to host this blog on it. Mainly because Google does not index github.io blogs.

The setup is based on [this article](https://johnfraney.ca/blog/build-deploy-static-site-dokku/).

## Vitepress setup

### 1. create an empty `.static` file to tell Dokku to serve static files

### 2. define buildpacks
Create a file called `.buildpacks` with the following content:
```
https://github.com/heroku/heroku-buildpack-nodejs.git
https://github.com/dokku/buildpack-nginx.git
```

## Dokku deploy

## Prerequisites

- A Dokku server set up and running.
- SSH access to your Dokku server.

## Steps on Dokku server

### 1. Create dokku app and configure environment variables
```bash
dokku apps:create blog
dokku config:set blog --no-restart NODE_ENV=production NGINX_ROOT=_site
```

### 2. Let's Encrypt
If not already installed:
```bash 
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku letsencrypt:set --global email your@email.address  
dokku letsencrypt:cron-job --add
```
Enable SSL for the app:
```bash
dokku letsencrypt:enable blog
```

## Steps on local machine
### 1. Add dokku as a remote on local machine
```bash
git remote add dokku dokku@your-dokku-server:blog
```

### 2. Push to dokku server
```bash
git push dokku main
```

<Comment />
