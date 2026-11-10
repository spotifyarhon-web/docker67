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

SQLite хранится в именованном volume `counter_data`, подключённом к `/data`.
Проверка сохранения:

```sh
docker compose down
docker compose up -d
curl http://localhost:8080/
```

Счётчик должен сохранить значение. `docker compose down -v` удаляет volume
и данные, поэтому для этой проверки его использовать нельзя.

Результаты проверки — в `report.txt`.
