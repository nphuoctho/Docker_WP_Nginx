DPRESS 
===
# Introduction

## Quick Overview
Let's see how easy it is install:

### 1.  Clone *dpress* inside `/var/www` directory

``` bash
git clone https://gitlab.com/hientt53/dpress.git 
```
### 2. Enter *dpress* and rename `env-exmaple` to `.env` .
``` bash 
cp env-example .env
```
### 3. Run your containers:
``` bash 
docker-compose up -d
```
### 4. Open your browser and visit localhost: `http://locahost:8888` .
``` bash 
That's  it! enjoi :)
```

## Getting Started 

### Requirements
* [Git](https://git-scm.com/downloads)
* [Docker](https://www.docker.com/community-edition) `>= 18`
* [Docker compose](https://docs.docker.com/compose/install)
* [Nginx](https://nginx.org/en/download.html?_ga=2.131347343.1297889217.1528973014-1047148726.1528973014)
### Installation

### 1.  Clone *dpress* inside `/var/www` directory

``` bash
git clone https://gitlab.com/hientt53/dpress.git 
```
### 2. Rename *dpress* folder to domain name
With multiple site your structure should look like this: 
``` bash 
+ domain-A  (dpress source)
+ domain-B  (dpress srouce)
+ domain-C  (dpress srouce)
```
Edit your web server sites configuration.
``` bash
cp env-example .env
```
At the top, change the `APP_NAME` variable with your folder name.
``` bash 
APPLICATION=domain-A
```
Second, change the `NGINX_PORT` variable
``` bash 
NGINX_PORT=9000
``` 
### 3. Run your containers:
``` bash 
docker-compose up -d
```
Now your site avariable at: `http://locahost:9000`
### 4: Setup nginx server name
In nginx.conf change below: 
1. `example.local` to your domain name 
``` nginx
        server {
                listen 80;
                server_name example.local; # change this domain
                ...
        }
        ...
        server {
                server_name www.example.local; # change this domain
                return 301 $scheme://example.local$request_uri; #change this domain
        }
```
2. `proxy_pass http://127.0.0.1:8888;` to your `NGINX_PORT` 
``` nginx
        server {
                ...
                proxy_pass http://127.0.0.1:9000; # change this port
                ...
        }
```
Link your `nginx.conf` to nginx config
```
ln -s $PWD/nginx.conf /etc/nginx/site-enables/domain_name
```
Verify nginx config is correct
```
nginx -t
# output 
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
Reload nginx 
``` bash 
sudo /etc/init.d/nginx reload
```

### 5. Congratulation 

### Note:
*dpress* build in config mysql to use mininal memory. 

| Setting                    | Default    | Minimum  |                                        |
|----------------------------|------------|----------|----------------------------------------|
| innodb_buffer_pool_size    | 128M       | 5M       |                                        |
| innodb_log_buffer_size     | 1M         | 256K     |                                        |
| query_cache_size           | 1M         | 0        |                                        |
| max_connections            | 151        | 1        | (although 10 might be more reasonable) |
| key_buffer_size            | 8388608    | 8        |                                        |
| thread_cache_size          | (autosized | )        | 0                                      |
| host_cache_size            | (autosized | )        | 0                                      |
| innodb_ft_cache_size       | 8000000    | 1600000  |                                        |
| innodb_ft_total_cache_size | 640000000  | 32000000 |                                        |
| thread_stack               | 262144     | 131072   |                                        |
| sort_buffer_size           | 262144     | 32K      |                                        |
| read_buffer_size           | 131072     | 8200     |                                        |
| read_rnd_buffer_size       | 262144     | 8200     |                                        |
| max_heap_table_size        | 16777216   | 16K      |                                        |
| tmp_table_size             | 16777216   | 1K       |                                        |
| bulk_insert_buffer_size    | 8388608    | 0        |                                        |
| join_buffer_size           | 262144     | 128      |                                        |
| net_buffer_length          | 16384      | 1K       |                                        |
| innodb_sort_buffer_size    | 1M         | 64K      |                                        |
| binlog_cache_size          | 32K        | 4K       |                                        |
| binlog_stmt_cache_size     | 32K        | 4K       |                                        |
