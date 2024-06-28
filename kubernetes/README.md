## Задание 1
**Скачивание и установка minikube, kubectl**
1. 
```text
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```
2. Запуск minicube
```text
minikube start
```
3. Установка kubectl
```text
curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```
4. Проверка запуска кластера
```text
kubectl get po -A
kubectl get nodes
```
5. Меняем конфигируцию redis.yml для запуска redis без пароля. Требуется добавить переменную окружения *ALLOW_EMPTY_PASSWORD*
```
- name: ALLOW_EMPTY_PASSWORD
  value: "yes"
```
6. Запускаем контейнер: 
`kubectl apply -f ./config/redis.yml`
7. Запускаем Services
```
kubectl expose deployment/redis --port 6379
```
8. Проверяем IP и запуск Services
```
kubectl get svc
kubectl get po -o wide
```
9. Напишите команды kubectl для контейнера из предыдущего задания:
 - выполнения команды ps aux внутри контейнера: 
  `kubectl exec -it deployment/redis -- ps aux`
 - просмотра логов контейнера за последние 5 минут;
    ```
    kubectl logs deploy/redis
    kubectl logs --tail 200 deploy/redis
    ```
 - удаления контейнера;
  `kubectl delete deploy/redis`
 - проброса порта локальной машины в контейнер для отладки
  `kubectl port-forward deploy/redis 6379:9100`
