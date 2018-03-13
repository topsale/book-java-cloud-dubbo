# Nginx 部署

---

## docker-compose.yml

```
version: '3.1'
services:
  nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 80:80
    volumes:
      - /usr/local/docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
      - /usr/local/docker/nginx/nginx.conf:/etc/nginx/nginx.conf
      - /usr/local/docker/nginx/html:/usr/share/nginx
```