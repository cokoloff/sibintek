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