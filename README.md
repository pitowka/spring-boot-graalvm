Jednoduchy priklad na vytvorenie graalvm image podla
https://docs.spring.io/spring-boot/how-to/native-image/developing-your-first-application.html

- vytvorenie image (docker): ./gradlew bootBuildImage
- spustenie cez docker: docker run --rm -p 8080:8080 graalvm:0.0.1-SNAPSHOT (kde je image ulozeny; je niekde na webe?)

- nativny image (bez docker): ./gradlew nativeCompile (musi sa ale pouzi graalvm)
- ulozi sa do build/native/nativeCompile/
