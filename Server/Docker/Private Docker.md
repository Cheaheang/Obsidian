enable **DOCKER_BUILDKIT**(window)
```
$env:DOCKER_BUILDKIT=1
```
**CMD** to build docker-compose and 
```
 docker-compose build -d
```

```
docker-compose up
```
**CMD** to rebuild image
```bash
docker-compose up --build -d
```
**CMD** to go to the container 
```bash
docker exec -it shop_website sh
```
**CMD** to restart specific service
```bash
docker-compose restart webserver
```
**Tips:**
- In short: You write the **Dockerfile**, you build it to get an **Image**, and you run the **Image** to start a **Container**.