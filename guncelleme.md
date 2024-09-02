```console
 docker stop scout

 docker rm scout

 docker pull chasmtech/chasm-scout:latest

 docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout

```

```console


# bu komutla OK çıktısı aldıysak tamamdır.
curl localhost:3001

# logları kontrol edelim
docker logs scout
```
