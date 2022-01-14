## Nest.JS 도커라이징

1. 먼저 Dockerfile 을 생성한다.
2. NestJS는 node랑 다르게 build(ts->js)를 수행하기 때문에 파일 전체를 복사할 필요가 없어 빌드된 파일만 Docker Image에 넣어주기하면 된다.
3. node가 설치된 이미지 기준으로 빌드된 파일을 복사하여 만든다.

### Dockerfile
```
## base image for Step 1: Node 10
FROM node:14 AS builder
WORKDIR /app
## 프로젝트의 모든 파일을 WORKDIR(/app)로 복사한다
COPY . .
## Nest.js project를 build 한다
RUN npm install
RUN npm run build


# Step 2
## base image for Step 2: Node 10-alpine(light weight)
FROM node:14-alpine
WORKDIR /app
## Step 1의 builder에서 build된 프로젝트를 가져온다
COPY --from=builder /app ./
EXPOSE 8092
## application 실행
CMD ["npm", "run", "start:prod"]

```

### Docker build
- docker build -t hsnam/nest-docker:latest .
