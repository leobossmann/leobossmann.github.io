# leo's blog

based on [Mtillmann/mtillmann.github.io](https://github.com/Mtillmann/mtillmann.github.io)

which in turn is based on

based on [airene/vitepress-blog-pure](https://github.com/airene/vitepress-blog-pure)

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
`dokku letsencrypt:enable blog`

## Steps on local machine
### 1. Add dokku as a remote on local machine
`git remote add dokku dokku@your-dokku-server:blog`

### 2. Push to dokku server
`git push dokku main`

