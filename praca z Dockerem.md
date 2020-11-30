---
layout: page
title: Praca z systemem Docker
---

Wszystkie potrzebne programy będą dostarczane w postaci kontenerów dockera.

# Zacznij korzystać z Dockera

W celu pobrania oprogramowania docer na swój system przejdź do [strony](https://docs.docker.com/get-docker/).

Jeżli wszystko zainstalowało się prawidłowo wykonaj następujące polecenia:

1. Sprawdź zainstalowaną wersję

```{bash}
docker --version
```

2. Ściągnij i uruchom obraz `Hello World` i 

```{bash}
docker run hello-world
```

3. Przegląd ściągnietych obrazów:

```{bash}
docker image ls

docker images
```

4. Przegląd uruchomionych kontenerów:

```{bash}
docker ps 

docker ps -all
```

5. Zatrzymanie uruchomionego kontenera: 

```{bash}
docker stop <CONTAINER ID>
```

6. Usunięcie kontenera
```{bash}
docker rm -f <CONTAINER ID>
```

Polecam również [krótkie intro](https://medium.com/codingthesmartway-com-blog/docker-beginners-guide-part-1-images-containers-6f3507fffc98)
