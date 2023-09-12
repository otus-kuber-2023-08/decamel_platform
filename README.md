# decamel_platform
decamel Platform repository

# Выполнено ДЗ № 1

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - настроены рабочие потоки GithubActions
 - установлены **kubectl** и **minikube**
 - описан `kubernetes-intro/web/Dockerfile` для сервера раздачи статического контента на порту 8000
 - описан манифест `kubernetes-intro/web-pod.yml` включающий **init-container** для загрузки *html* содержимого и запуска образа для раздачи этого содержимого
 - протестирована работоспособность описанного манифеста при помощи `kubectl port-forward --address 0.0.0.0 pod/web 8000:8000`
 - Сгенерирован манифест `kubernetes-intro/frontend-pod.yml` при помощи `--dry-run=client` опции `kubectl run`
 - Выявлена причина неработоспособности **pod**`a с образом **Hipster Frontend**. В манифесте отсутствут объявления необходимых для работы сервиса переменных окружения с адресами других микросервисов
 - Исправлен манифест `frontend-pod.yml`, результат помещен в `kubernetes-intro/frontend-pod-healthy.yaml`
   
## web
### Как запустить проект:
-  Перейти в корень проекта
 - Развернуть **pod** с помощью `kubectl apply -f ./kubernetes-intro/web-pod.yml`
 - Выполнить команду `kubectl port-forward --address 0.0.0.0 pod/web 8000:8000` которая пробросит локальный порт машины на порт **pod**`a
### Как проверить работоспособность:
 - Перейти по ссылке http://localhost:8000/index.html

## Hipster Frontend
### Как запустить проект:
-  Перейти в корень проекта
 - Развернуть **pod** с помощью `kubectl apply -f ./kubernetes-intro/frontend-pod-healthy.yml`
### Как проверить работоспособность:
- Удостовериться что **pod** находится в состоянии `Running`, выполнив команду `kubectl get pod frontend`
 

## PR checklist:
 - [x] Выставлен label с темой домашнего задания
 
 ### Разберитесь почему все **pod** в namespace kube-system восстановились после удаления 
 
 - `coredns` восстанавливается поскольку контролируется с помощью ресурста `ReplicaSet` со значением поддерживаемых экземпляров 1
 - `kube-proxy` запускается на каждой ноде, поскольку разворачивается в `DaemonSet`, поэтому кластер приводит ноду в требуемое состояние
 - `apiserver`, `scheduler`, `controller-manager` управляется `Node` напрямую
 
 ### Выясните причину, по которой **pod** `frontend` находится в статусе `Error`
 - В манифесте отсутствовали необходимые переменные окружения, добавление этих переменных позволяет **pod**`y перейти в статус `Running`