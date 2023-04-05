## ТЗ

Сделать сервис по работе с балансами. Возьмите скелетон сервиса по ссылке: <https://github.com/list-family/backend-service-skeleton>.
Код предоставляется для сокращения затрат на boilerplate, его можно слегка переструктурировать и исправлять ошибки, если это уместно.

Нужно реализовать:

- REST API добавление пользователя
- REST API добавление транзакции (DEPOSIT или WITHDRAW)
- REST API получить информацию о транзакции
- REST API получить текущий баланс пользователя (без селекта всех транзакций) или баланс за дату (например, запрос "сколько у Васи было денег 5.05.2022 00:00")

Ваша реализация должна проходить тесты `pytest՝.

- python3
- sqlalchemy
- postgresql

Дополнительно:

- учесть, что сервис будет запускаться в k8s.
- учесть, что архитектура должна гарантировать обработку транзакции ровно 1 раз. Какие требования это накладывает на сам сервис и соседние сервисы?
- описать (можно не реализовывать), как можно реализовать уведомление других сервисов о транзакциях. Например, уведомить рекламный движок, который работает при наличии средств на счету.
- тезисно перечислить, какие инструменты можно применить для контроля качества работы сервиса
- гарантировать, что баланс пользователя не может быть отрицательным

## Настройка для vscode

- tasks.json:

```json
{
    "tasks": [
        {
            "type": "docker-run",
            "label": "docker-run: debug",
            "dependsOn": [
                "docker-build"
            ],
            "python": {
                "module": "app",
            }
        },
        {
            "label": "docker-build",
            "type": "docker-build",
            "dockerBuild": {
                "context": "${workspaceFolder}",
                "dockerfile": "${workspaceFolder}/docker/Dockerfile",
                "tag": "app:latest"
            }
        }
    ]
}```

- launch.json

```json
{
    "configurations": [
        {
            "name": "Docker: Python",
            "type": "docker",
            "request": "launch",
            "preLaunchTask": "docker-run: debug",
            "python": {
                "pathMappings": [
                    {
                        "localRoot": "${workspaceFolder}",
                        "remoteRoot": "/app"
                    }
                ],
                "projectType": "general"
            }
        }
    ]
}
```