# Лабораторная работа №4 по курсу «Технологии сетевого взаимодействия»
# Выполнил студент группы М80-209М-23 Брежнев В.А.
Задание - задеплоить Nginx со статической страницей

Структура проекта
Dockerfile Файл для сборки Docker-образа
README.md Инструкция по развёртыванию
deployment.yaml Манифест Kubernetes для Deployment
index.html HTML-файл для статической страницы
service.yaml  Манифест Kubernetes для Service
configmap.yaml Манифест Kubernetes для ConfigMap

Инструкция по запуску и развёртыванию на Kubernetes:
Перед началом убедиться, что у вас установлены следующие инструменты:
Docker 
kubectl 
k3d 

1. Клонирование репозитория
git clone <URL-репозитория>
2. Перейти в папку с проектом
cd my-nginx-project
3. Создание кластера k3d
k3d cluster create test-cluster --port '30001:30001'
4. Сборка Docker-образа
docker build -t my-nginx:v1 .
5. Импорт образа в k3d
k3d image import my-nginx:v1 --cluster test-cluster
6. Применение манифестов Kubernetes
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
7. Проверка работы
kubectl get pods
8. Открыть браузер и перейти по адресу:
http://localhost:30001
