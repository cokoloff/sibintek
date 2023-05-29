# 1 Задание
Установить Docker на локальную машину, если он еще не установлен.
Создать директорию для проекта и перейти в эту директорию в командной строке.
Создать файл Dockerfile для нашего проекта. В нем должно быть следующее:
```dockerfile
  # Начинаем с базового образа nginx
  FROM nginx

  # Копируем файл default.conf в директорию веб-сервера
  COPY default.conf /etc/nginx/conf.d/

  #Монтируем каталог
  VOLUME /usr/share/nginx/html

  #Пробрасываем порт вовне
  EXPOSE 8080

  # Запускаем веб-сервер при старте контейнера
  ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

Создать файл default.conf для нашего nginx. В нем должно быть следующее:
```conf
  server {
    listen       80;
    server_name  dockerhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

  }
```

Создать директорию для проекта www, в ней необходимо создать файл index.html с содержимым:
```html
  Hello Docker!
```

Запустить сборку образа, в терминале, находясь в основной директории проекта, выполнив команду: 
```bash
  docker build -t dockerhost . 
```

Создать контейнер на базе созданного образа с помощью команды:
```bash
  docker run -v $(pwd):/usr/share/nginx/html -d -p 8080:80 --name dockerhost-container dockerhost
```

Проверить, что контейнер работает, перейдя по адресу http://dockerhost:8080 в браузере. 
Если все прошло успешно, то должно отобразиться «Hello Docker!».

# 2 Задание
Так как мы изначально примонтировали необходимый файл через каталог, мы можем иго изменить как нам хочеться.
Открыть файл, в котором находится строка "Hello Docker!". и заменить ее на "Hello Sibintek!"
Проверяем, что контейнер работает, перейдя по адресу http://dockerhost:8080 в браузере. 
Обновляем кэш браузена клавишей F5, должно отобразиться «Hello Docker!».

# 3 Задание
Установить Minikube, запустить его командой:
```bash
  minikube start
```

Переключаемся на контекст миникуба:
```bash
  kubectl config use-context minikube
```

Создаем отдельное пространство:
```bash
  kubectl create namespace dockerhost
```

Создаем новый файл dockerhost.yaml следующего содержания:
```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: dockerhost
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: dockerhost
    template:
      metadata:
        labels:
          app: dockerhost
      spec:
        containers:
          - name: dockerhost-container
            image: nginx
            imagePullPolicy: "IfNotPresent"
            ports:
              - containerPort: 80
            volumeMounts:
              - name: index-config
                mountPath: /usr/share/nginx/html/index.html
                subPath: index.html
        volumes:
          - name: index-config
            configMap:
              name: index-config
              items:
                - key: index.html
                  path: index.html

  ---

  apiVersion: v1
  kind: Service
  metadata:
    name: dockerhost
    labels:
      app: dockerhost
  spec:
    type: NodePort
    ports:
      - name: http
        port: 80
        protocol: TCP
    selector:
      app: dockerhost

  ---

  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: index-config
  data:
    index.html: |
      Hello Docker!
```
Применяем наш манифест:
```bash
  kubectl apply -f dockerhost.yaml -n dockerhost
```

Проверяем все ли запустилось:
```bash
  kubectl get pods -n dockerhost
```

Проверяем на каком порту запустился сервис:
```bash
  kubectl get svc -n dockerhost
```

Пробуем обратиться к хосту по открытому порту сервиса:
```bash
  curl nodehost:32667
```