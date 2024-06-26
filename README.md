# Домашнее задание к занятию «`Хранение в K8s. Часть 2`» - `Живарев Игорь`

### Цель задания

В тестовой среде Kubernetes нужно создать PV и продемострировать запись и хранение файлов.

------

### Чеклист готовности к домашнему заданию

1. Установленное K8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным GitHub-репозиторием.

------

### Дополнительные материалы для выполнения задания

1. [Инструкция по установке NFS в MicroK8S](https://microk8s.io/docs/nfs). 
2. [Описание Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/). 
3. [Описание динамического провижининга](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/). 
4. [Описание Multitool](https://github.com/wbitt/Network-MultiTool).

------

### Задание 1

**Что нужно сделать**

Создать Deployment приложения, использующего локальный PV, созданный вручную.

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
2. Создать PV и PVC для подключения папки на локальной ноде, которая будет использована в поде.
3. Продемонстрировать, что multitool может читать файл, в который busybox пишет каждые пять секунд в общей директории. 
4. Удалить Deployment и PVC. Продемонстрировать, что после этого произошло с PV. Пояснить, почему.
5. Продемонстрировать, что файл сохранился на локальном диске ноды. Удалить PV.  Продемонстрировать что произошло с файлом после удаления PV. Пояснить, почему.
5. Предоставить манифесты, а также скриншоты или вывод необходимых команд.

------

### Задание 2

**Что нужно сделать**

Создать Deployment приложения, которое может хранить файлы на NFS с динамическим созданием PV.

1. Включить и настроить NFS-сервер на MicroK8S.
2. Создать Deployment приложения состоящего из multitool, и подключить к нему PV, созданный автоматически на сервере NFS.
3. Продемонстрировать возможность чтения и записи файла изнутри пода. 
4. Предоставить манифесты, а также скриншоты или вывод необходимых команд.

------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

---


## Ответ


### Задание 1. Создать Deployment приложения, использующего локальный PV, созданный вручную.

Создаём `Deployment` приложения, состоящего из контейнеров _busybox_ и _multitool_
![manifest](img/k8s-07_01.png)

Создаём `PV` для подключения папки на локальной ноде
![pv](img/k8s-07_02.png)

Создаём `PVC` для подключения папки на локальной ноде
![pvc](img/k8s-07_03.png)

Подключаемся к `Pod` с _multitool_ и проверяем возможность чтения из файла
![multitool](img/k8s-07_04.png)

Удаляем `Deployment` и `PVC`, проверяем статус `PV`
![delete](img/k8s-07_05.png)

Вывод: после удаления `PVC`, `PV` сменил статус с _Bound_ на _Released_, так как больше не свзан с `PVC`.

Демонстрация сохранности файла на локальной ноде
![file](img/k8s-07_06.png)

Удаление `PV`
![pv_delete](img/k8s-07_07.png)

Файл на месте и доступен для чтения
![pv_delete](img/k8s-07_08.png)

Вывод: политика `Reclaim Policy: Retain` сохраняет файлы даже после удаления `PV`.

Манифесты:
[deployment](/deploy_busybox_multitool.yml)  
[pvc](/pvc-one.yml)  
[pv](/pv-one.yml)



### Задание 2. Создать Deployment приложения, которое может хранить файлы на NFS с динамическим созданием PV.

Включаем `NFS` сервер на `MicroK8S`
![nfs](img/k8s-07_09.png)

Создаём `Deployment` приложения состоящий из _multitool_, и подключаем к нему `PV`, созданный автоматически на сервере `NFS`
![manifest](img/k8s-07_10.png)

Демонстрация возможности чтения и записи файла изнутри пода
![pod](img/k8s-07_11.png)


Манифесты:
[deployment](/deploy_multitool.yml)  
[pvc](/pvc-nfs.yml)


---