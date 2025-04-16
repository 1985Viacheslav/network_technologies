# Лабораторная работа №6 по курсу «Технологии сетевого взаимодействия»
# Выполнил студент группы М80-209М-23 Брежнев В.А.

Задание - Создать Stateful приложение в k8s (со StatefulSet),  которое будет работать со своим PV (можно статическим). Приложение должно быть с фронтендом и бэкендом с правами хранилища и использует механизмы секретов паролей (использует любой из механизмов который был на занятии).

Создано приложение, развёрнутое при помощи Kubernetes, работающее в качестве фронтенда с Nginx, в качестве бэкенда - с MySQL. Оно отображает приветственную страницу с текстом "Welcome to nginx!". Приложение развертывается в локальном кластере Kubernetes (k3d) и доступно через браузер.

Инструкция по развертыванию:
1. Клонирование репозитория git clone <URL-репозитория>
2. Перейти в папку с проектом cd my-stateful-project
3. Создать кластер с проброской порта k3d cluster create my-cluster --port 30002:80@loadbalancer
4. Применить манифесты:
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f secret.yaml
kubectl apply -f statefulset.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
5. Проверка работы:
kubectl get pods
kubectl get pv,pvc
6. Открыть бэкенд приложения:

kubectl exec -it <mysql-pod-name> -- mysql -u root -p, например: если имя mysql-0:
kubectl exec -it mysql-0 -- mysql -u root -p
Вссести пароль, который был зашифрован base64 (для удобства указан примечанием в secret.yaml) и выполнить команду MySQL:
SHOW DATABASES;

7. Открыть фронтенд приложения:
Перейти на страницу
http://localhost:30002/
если не находит файл можно воспользоваться либо форвардингом порта:
kubectl port-forward svc/frontend 8080:80
и перейти:
http://localhost:8080/
или временно использовать прокси:
kubectl proxy --port=8080
и перейти:
http://localhost:8080/api/v1/namespaces/default/services/frontend/proxy/
