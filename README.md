!(./images/auth.png)

# Установка
## 1) Установить в k8s ingress-nginx (опционально, если отсутствует)
```kubectl create namespace m && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace m -f nginx/0_OPT_nginx_ingress-25239-20146a.yaml```

## 2) Установить сервис auth в отдельный ns

```kubectl create ns auth && kubectl config set-context --current --namespace=auth && cd auth && skaffold run && kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission```

## 3) Создать отдельный namespace

```kubectl create ns msusers-ns```

## 4) Развернуть БД в k8s

```helm install ./helm-chart --generate-name```

## 5) Провести миграцию данных в БД (опционально, в первый раз!)

```kubectl apply -f migr```

## 6) Развернуть приложение в k8s + ingress auth

```kubectl apply -f k8s```

# Tecтирование

Пример вызова:
* `curl arch.homework/users`

## Postman

Рекомендуется установить расширение для newman `npm install -g newman-reporter-htmlextra`

* Коллекция: https://www.postman.com/alexkorkhov/public/collection/kgjkovp/otus-users-auth
* Сценарий: `https://www.postman.com/alexkorkhov/public/collection/fjvjm00/otus-users-auth-tests`
* Проверка коллекции: `newman run postman-auth/OTUS-USERS-AUTH-TESTS.postman_collection.json -r htmlextra` 
* Результат выполнения: `postman-auth/newman/OTUS-USERS-AUTH-TESTS-2025-06-15-18-24-27-779-0.html`

# Описание
Сервер на Java представляет эндпоинты:
* Admin (для администратора)
  * `(GET) /users` возвращает список пользователей
  * `(GET) /user/{id}` возвращает данные по id пользователя
  * `(POST) /user` создает пользователя
  * `(PUT) /user/{id}` обновляет данные по id пользователя
  * `(DELETE) /user/{id}` удаляет данные по id пользователя
* User (для пользователя)
  * `(GET) /user/me` возвращает данные пользователя (требуется авторизация)
  * `(PUT) /user/me` обновляет данные пользователя (требуется авторизация)
* Auth (методы аутентификации)
  * `(POST) /register` - регистрация пользователья
  * `(POST) /login` - вход (логин) в систему
  * `(GET) /logout` - выход (логаут) из системы
  * `(GET) /signin` - требование авторизоваться, используется основным приложением
  * `(GET) /auth` - аутентификация, используется основным приложением 
