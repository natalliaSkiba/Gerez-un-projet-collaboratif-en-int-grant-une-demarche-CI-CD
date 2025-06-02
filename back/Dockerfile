#Задает базовый образ — maven версии 3.9.9, основанный на JDK 17 от Eclipse Temurin на базе Ubuntu Focal.
FROM maven:3.9.9-eclipse-temurin-17-focal AS build

#Устанавливает рабочую директорию /workspace, чтобы последующие команды выполнялись именно там.
WORKDIR /workspace

#Копирует файлы проекта из моего локального каталога внутрь контейнера
COPY pom.xml /workspace
COPY src /workspace/src

# clean package — удаляет старые артефакты и собирает .jar
#-DskipTests — пропускает юнит-тесты
# -B — batch mode (без интерактивных запросов, удобно в CI/CD)
RUN mvn -B -f pom.xml clean package -DskipTests

#Указывает, что для запуска будет использоваться легкий образ JDK 17 от Eclipse Temurin.
# Где найти:
#Docker Hub: eclipse-temurin — ищи тег 17, например eclipse-temurin:17-jdk-jammy.
FROM eclipse-temurin:17-jdk-jammy

#Копирует собранный .jar файл из предыдущего (build) слоя во второй образ (где только JDK), и называет его app.jar.
COPY --from=build /workspace/target/*.jar app.jar

#Открывает порт 8080 внутри контейнера, чтобы приложение стало доступно снаружи (на этом порту обычно работает Spring Boot).
EXPOSE 8080

#Указывает, какую команду выполнять при запуске контейнера — запускает  Spring Boot-приложение.
ENTRYPOINT ["java", "-jar","app.jar"]
