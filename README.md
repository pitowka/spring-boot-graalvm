Jednoduchy priklad na vytvorenie graalvm image podla
https://docs.spring.io/spring-boot/how-to/native-image/developing-your-first-application.html

- vytvorenie image (docker): ./gradlew bootBuildImage
- spustenie cez docker: docker run --rm -p 8080:8080 graalvm:0.0.1-SNAPSHOT (kde je image ulozeny; je niekde na webe?)

- nativny image (bez docker): ./gradlew nativeCompile (musi sa ale pouzi graalvm)
- ulozi sa do build/native/nativeCompile/

- export docker image: docker save graalvm:0.0.1-SNAPSHOT -o graalvm.docker.tar
- import docker image: docker load -i graalvm.docker.tar

- urobit docker compose ak chcem mat aj ine systemy (napr. postgres)

- docker pre appku
```
FROM scratch
COPY build/native/nativeCompile/graalvm /app
ENTRYPOINT ["/app"]
```
- a potom: docker build -t graalvm:latest .

- vysledny docker-compose.yml asi takto:
```
services:
  app:
    image: moje-app:latest
    build:
      context: .
      dockerfile: Dockerfile
    depends_on:
      - postgres
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/mojadb
      SPRING_DATASOURCE_USERNAME: user
      SPRING_DATASOURCE_PASSWORD: pass
    ports:
      - "8080:8080"

  postgres:
    image: postgres:16
    environment:
      POSTGRES_DB: mojadb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

