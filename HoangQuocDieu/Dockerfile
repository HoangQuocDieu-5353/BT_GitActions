# Bước 1: Build ứng dụng với JDK 25
FROM eclipse-temurin:25-jdk AS build
WORKDIR /app

# Copy các file cấu hình Maven Wrapper trước
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
# Cấp quyền thực thi cho mvnw
RUN chmod +x mvnw
# Tải các thư viện cần thiết trước (cache)
RUN ./mvnw dependency:go-offline

# Copy code nguồn và build
COPY src ./src
RUN ./mvnw clean package -DskipTests

# Bước 2: Chạy ứng dụng với JRE 25 (nhẹ hơn)
FROM eclipse-temurin:25-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]