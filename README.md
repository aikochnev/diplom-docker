# Docker-приложение

Каталог docker/ содержит приложение, Dockerfile, Compose-конфигурацию и GitHub Actions workflow для сборки и публикации Docker image.

Docker-часть проекта отвечает за:

    упаковку приложения в Docker image;

    локальный запуск контейнера;

    публикацию image в Yandex Container Registry;

    автоматический deployment на виртуальные машины;

    проверку работы приложения.

Docker Compose описывает конфигурацию одного или нескольких сервисов в compose.yaml и позволяет запускать приложение воспроизводимым способом.

## Структура каталога
```
docker/
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── compose.yaml
├── Dockerfile
├── index.html
└── README.md

```
## Назначение файлов:
Файл	                                 Назначение
Dockerfile	                Инструкция сборки Docker image
compose.yaml	                Конфигурация локального запуска контейнера
index.html	                Содержимое демонстрационного веб-приложения
.github/workflows/ci-cd.yml	GitHub Actions CI/CD pipeline
README.md	                Документация Docker-подсистемы

## Общая схема работы
```
index.html
    ↓
Dockerfile
    ↓
Docker image
    ↓
Yandex Container Registry
    ↓
GitHub Actions
    ↓
SSH на web-b и web-d
    ↓
docker pull
    ↓
docker run
    ↓
Приложение доступно по HTTP
```
Локально приложение можно запускать через Docker Compose:
```
compose.yaml
    ↓
docker compose up
    ↓
Docker container
    ↓
http://localhost
```

## Dockerfile — это сценарий сборки Docker image.

Docker последовательно выполняет инструкции из Dockerfile, создавая слои image. В image попадают:

    базовый образ;

    веб-сервер;

    файлы приложения;

    настройки запуска;

    открываемый порт.

Dockerfile описывает не конкретный контейнер, а шаблон, из которого контейнеры создаются.

## Контейнер приложения

После сборки image запускается контейнер:
```
Docker image
    ↓
Docker container
```
Image является неизменяемым шаблоном, а container — запущенным экземпляром image.

Проверить запущенные контейнеры:

docker ps

Проверить все контейнеры:

docker ps -a

## Файл index.html содержит пользовательский интерфейс демонстрационного приложения.

## Файл compose.yaml описывает локальный запуск приложения через Docker Compose.

Docker Compose позволяет описать сервис, его image или build context, порты и политику перезапуска в одном YAML-файле.

## Файл docker/.github/workflows/ci-cd.yml содержит автоматический CI/CD pipeline.

Workflow запускается после push в ветку main.

Общая схема:
```
git push
    ↓
GitHub Actions
    ↓
checkout
    ↓
Docker Buildx
    ↓
docker login
    ↓
docker build
    ↓
docker push
    ↓
SSH web-b
    ↓
SSH web-d
```
Docker поддерживает сборку image в GitHub Actions через BuildKit и Docker build/push actions.



<img src = "img/a-01.png" width = 100%>

### После пересоздания VM

cd ~/diplom/terraform
terraform destroy
terraform apply

cd ~/diplom/ansible
ansible-playbook bootstrap.yml

<img src = "img/a-02.png" width = 100%>