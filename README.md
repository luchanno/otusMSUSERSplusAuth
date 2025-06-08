# Установка
## 1) Установить в k8s ingress-nginx (опционально, если отсутствует)
```kubectl create namespace m && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace m -f nginx/0_OPT_nginx_ingress-25239-20146a.yaml```

## 2) Создать отдельный namespace

```kubectl create ns msusers-ns```

## 3) Развернуть БД в k8s

```helm install ./helm-chart --generate-name```

## 4) Провести миграцию данных в БД (опционально, в первый раз!)

```kubectl apply -f migr```

## 5) Развернуть приложение в k8s

```kubectl apply -f k8s```

# Tecтирование

Пример вызова:
* `curl arch.homework/users`

## Postman

* Коллекция: `https://www.postman.com/alexkorkhov/public/collection/aquuwr3/otus-users`
* Проверка коллекции: `newman run postman/OTUS-USERS.postman_collection.json`
* Скриншот выполнения: `postman/newman.JPG`

# Описание
Сервер на Java представляет эндпоинты:
* `(GET) /users` возвращает список пользователей
* `(GET) /user/{id}` возвращает данные по id пользователя
* `(POST) /user` создает пользователя
* `(PUT) /user/{id}` обновляет данные по id пользователя
* `(DELETE) /user/{id}` удаляет данные по id пользователя
