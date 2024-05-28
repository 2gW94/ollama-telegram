<div align="center">
  <br>
  <a href="">
    <img src="res/github/ollama-telegram-readme.png" width="200" height="200">
  </a>
  <h1>🦙 Ollama Telegram Бот</h1>
  <p>
    <b>Общайтесь с вашим LLM, используя бота в Telegram!</b><br>
    <b>Не стесняйтесь вносить свой вклад!</b><br>
  </p>
  <br>
  <p align="center">
    <img src="https://img.shields.io/docker/pulls/ruecat/ollama-telegram?style=for-the-badge">
  </p>
  <br>
</div>

## Особенности
Вот особенности, которые вы получаете "из коробки":

- [x] Полностью докеризованный бот
- [x] Потоковая передача ответов без ограничения частоты с **методом Предложение за Предложением**
- [x] Упоминание [@] бота в группе для получения ответа

## План работ
- [x] Конфигурация Docker и автоматические теги от [StanleyOneG](https://github.com/StanleyOneG), [ShrirajHegde](https://github.com/ShrirajHegde)
- [x] История и `/reset` от [ShrirajHegde](https://github.com/ShrirajHegde)
- [ ] Добавить больше функций, связанных с API [Редактор системных подсказок, Получение версии Ollama и т. д.]
- [ ] Интеграция с Redis DB
- [ ] Обновление интерфейса бота

## Предварительные требования
- [Токен Telegram-Bot](https://core.telegram.org/bots#6-botfather)

## Установка (без Docker)
+ Установите последнюю версию [Python](https://python.org/downloads)
+ Клонируйте репозиторий
    ```
    https://github.com/2gW94/ollama-telegram
    ```
+ Установите зависимости из requirements.txt
    ```
    pip install -r requirements.txt
    ```
+ Введите все значения в .env.example

+ Переименуйте .env.example -> .env

+ Запустите бота

    ```
    python3 run.py
    ```

## Установка (Docker Image)
Официальное изображение доступно на dockerhub: [ruecat/ollama-telegram](https://hub.docker.com/r/ruecat/ollama-telegram)

+ Скачайте файл [.env.example](https://github.com/ruecat/ollama-telegram/blob/main/.env.example), переименуйте его в .env и заполните переменные.
+ Создайте `docker-compose.yml` (необязательно: раскомментируйте часть файла с GPU, чтобы включить ускорение Nvidia GPU)

    ```yml
    version: '3.8'
    services:
      ollama-telegram:
        image: ruecat/ollama-telegram
        container_name: ollama-telegram
        restart: on-failure
        env_file:
          - ./.env
      
      ollama-server:
        image: ollama/ollama:latest
        container_name: ollama-server
        volumes:
          - ./ollama:/root/.ollama
        
        # Раскомментируйте, чтобы включить NVIDIA GPU
        # В противном случае работает только на CPU:
    
        # deploy:
        #   resources:
        #     reservations:
        #       devices:
        #         - driver: nvidia
        #           count: all
        #           capabilities: [gpu]
    
        restart: always
        ports:
          - '11434:11434'
    ```

+ Запустите контейнеры

    ```sh
    docker compose up -d
    ```

## Установка (Создание собственного образа Docker)
+ Клонируйте репозиторий
    ```
    git clone https://github.com/ruecat/ollama-telegram
    ```

+ Введите все значения в .env.example

+ Переименуйте .env.example -> .env

+ Запустите ОДНУ из следующих команд docker compose, чтобы начать:
    1. Чтобы запустить ollama в контейнере Docker (необязательно: раскомментируйте часть файла docker-compose.yml с GPU, чтобы включить ускорение Nvidia GPU)
        ```
        docker compose up --build -d
        ```

    2. Для запуска ollama из установленного локально экземпляра (в основном для **MacOS**, так как образ Docker пока не поддерживает ускорение Apple GPU):
        ```
        docker compose up --build -d ollama-telegram
        ```

## Конфигурация окружения
|     Параметр     |                                                      Описание                                                      | Обязательный? | Значение по умолчанию |                          Пример                           |
|:-----------------:|:---------------------------------------------------------------------------------------------------------------------:|:-------------:|:---------------------:|:---------------------------------------------------------:|
|      `TOKEN`      | Ваш **токен Telegram бота**.<br/>[[Как получить токен?]](https://core.telegram.org/bots/tutorial#obtain-your-bot-token) |      Да       |       `yourtoken`      |               MTA0M****.GY5L5F.****g*****5k               |
|    `ADMIN_IDS`    |                Идентификаторы пользователей Telegram администраторов.<br/>Они могут изменять модель и управлять ботом.                |      Да       |                       | 1234567890<br/>**ИЛИ**<br/>1234567890,0987654321, и т. д. |
|    `USER_IDS`     |                     Идентификаторы пользователей Telegram обычных пользователей.<br/>Они могут только общаться с ботом.                      |      Да       |                       |   1234567890<br/>**ИЛИ** 1234567890,0987654321, и т. д.   |
|    `INITMODEL`    |                                                      LLM по умолчанию                                                      |      Нет      |        `llama2`        |        mistral:latest<br/>mistral:7b-instruct         |
| `OLLAMA_BASE_URL` |                                                  Ваш URL OllamaAPI                                                   |      Нет      |                       |          localhost<br/>host.docker.internal           |
|   `OLLAMA_PORT`   |                                                  Порт вашего OllamaAPI                                                  |      Нет      |         11434         |                                                       |
|   `TIMEOUT`       |                                                  Время ожидания в секундах для генерации ответов                      |      Нет      |         3000          |                                                       |


## Благодарности
+ [Ollama](https://github.com/jmorganca/ollama)

## Используемые библиотеки
+ [Aiogram 3.x](https://github.com/aiogram/aiogram)
