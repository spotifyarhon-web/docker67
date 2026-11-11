# docker67

Запуск:

```sh
cp .env.example .env
docker compose up -d --build
```

В `.env` можно изменить `APP_PORT` (порт на компьютере) и `GREETING`.
Внутри контейнера приложение использует порт 8000.

Проверка (если меняли порт, замените 8080):

```sh
curl http://localhost:8080/
curl -X POST http://localhost:8080/increment
docker compose logs app
```

Compose запускает приложение и Redis. Порт Redis доступен внутри сети Compose.
Redis записывает данные в именованный volume `redis_data`, подключённый к `/data`.
Проверка сохранения:

```sh
docker compose down
docker compose up -d
curl http://localhost:8080/
```

Счётчик должен сохранить значение. `docker compose down -v` удаляет volume
и данные, поэтому для этой проверки его использовать нельзя.

Проверка недоступности Redis:

```sh
docker compose stop redis
curl -i http://localhost:8080/healthz
curl -i http://localhost:8080/readyz
curl -i -X POST http://localhost:8080/increment
docker compose start redis
```

При остановленном Redis `/healthz` возвращает 200, а `/readyz` и
`POST /increment` возвращают 503. После запуска Redis дождитесь, пока
`/readyz` снова вернёт 200. Затем `GET /` покажет сохранённое значение,
а следующий POST увеличит его на 1.

Результаты проверки — в `report.txt`.
