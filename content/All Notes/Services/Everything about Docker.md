# Everything about Docker
31-03-2025
Tags: #docker #services 

## [[Install Docker]]

## Các lệnh cơ bản và thường dùng trong Docker

### Login to Docker Hub

Login to Docker
```
docker login -u (username)
```

Push the image
```
dọcker push (username)/(image-name)
```

Search image
```
docker search (image)
```

Pull image
```
docker pull (image)
```
### Images

Build from Docker file
```
docker build -t (image-name) .
```

List images
```
docker images
```

Delete a image
```
docker rmi (image-name)
```

### Container
Create a container from image + custom name
```
docker run --name (container-name) (image-name)
```

Run a container with port
```
docker run -p (host):(container) (image-name)
```

Run container in backgroud
```
docker run -d (image-name)
```

Start/stop container
```
docker start/stop (container)
```

Remove a docker container
```
docker rm (container)
```

Open a shell in the container
```
docker exec -it (container) sh
```

Check logs container
```
docker logs (container)
```

List all container
```
docker ps --all
```

Check usage của container
```
docker stats
```
