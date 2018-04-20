# GitLab Runner Docker

---

## docker-compose.yml

```
version: '3.1'
services:
  gitlab-runner:
    restart: always
    image: gitlab/gitlab-runner
    container_name: gitlab-runner
    volumes:
      - /usr/local/docker/runner/config:/etc/gitlab-runner
      - /var/run/docker.sock:/var/run/docker.sock
```

## 注册 Runner

```
docker exec -it gitlab-runner gitlab-runner register
```