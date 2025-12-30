**Port Binding**
Docker requires port bending to enable communication between the container and the host machine. This is necessary because Docker containers are isolated environments by default, meaning the applications running inside them aren’t directly accessible from the host machine or external networks. Port bending allows traffic from the host to be forwarded to the container, enabling external access to the application inside the container.

How to bending port: 
```docker
docker run -d -p 9000:80 nginx:1.23
```

**In docker file**
```d
FROM node:19-alphine

## COPY <src> on our container <dest> in the container
COPY package.json /app/
COPY src /app/
## WORKDIR = cd
WORKDIR app/

RUN npm install

CMD [ "node", "server.js" ]
```
**Docker Process**
Dockerfile ->(build) Image -> (run) Container

```d
docker build -t node-app:1.0 .

docker run -d -p 3000:3000 node-app:1.0
```


