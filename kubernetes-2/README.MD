## Выполнение заданий
1. Создайте свой кластер с помощью kubeadm.
  - Добавляем ключи и репозитории kubeadm
    ```
    sudo apt update
    sudo apt install -y apt-transport-https ca-certificates curl gpg
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    ```
  - Устанавливаем пакеты:
    ```
    sudo apt update
    sudo apt-get install -y kubelet kubeadm kubectl containerd
    sudo apt-mark hold kubelet kubeadm kubectl
    ```
  - Включаем port-forwading на хостах
    ```
    sudo -i
    # Установка модуля br_netfilter
    modprobe br_netfilter
    # Включает форвардинг пакетов на хосте. Может понадобиться изменить в ручную файл /etc/sysctl.conf, расскоментировать строку: #net.ipv4.ip_forward=1
    echo "net.ipv4.ip_forward=1" >> /etc/systcl.conf
    echo "net.bridge.bridge-nf-call-iptables=1" >> /etc/sysctl.conf
    Другие варианты:
    sysctl -w net.ipv4.ip_forward=1
    nano /etc/sysctl.conf расскоментировать строку net.ipv4.ip_forward=1
    # Применение настроек
    sysctl -p /etc/sysctl.conf
    ```
  - Запускаем kubeadm
    ```
    sudo kubeadm init  \
    --apiserver-advertise-address=[Host_IP] \
    --pod-network-cidr [Netrowkd pod: 10.1.1.1/16] \
    --apiserver-cert-extra-sans=cluster_ip_address [Внешний IP адресс кластера] \
    --control-plane-endpoint=cluster_ip_address [Указываем балансировщик]
    ```
    ---
    ```
    sudo kubeadm init  \
    --apiserver-advertise-address=10.1.2.21 \
    --pod-network-cidr 172.16.0.0/16 \
    --apiserver-cert-extra-sans=158.160.96.120
    ```
  - Доп информация:
      - Сброс настроек kubeadm
      ```
      kubeadm reset
      ```
      - Отключение swap для успешного запуска
      ```
      swapoff -a
      ```
  - Добавление конфига
    ```
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    export KUBECONFIG=~/.kube/config
    ```

    - Данные для подключение к API Server:
    ```
    kubeadm join 10.1.2.21:6443 --token wvydaw.n24pajibew3wc9w3 \
    --discovery-token-ca-cert-hash sha256:4559699a31c808c75dcf49e99cbd92e881ff7751f35f321ebda79d1d83fcaa13
    ```
2. Запускаем и настраиваем вторую VM по аналогии с первой до пункта с запуском kudeadm, его запускать не требуется.
  VM подсоединяем в кластеру с указанием API-servera kubeadm и токена полученого при создание на первой машине.
  ```
  kubeadm join 10.1.2.21:6443 --token wvydaw.n24pajibew3wc9w3 \
    --discovery-token-ca-cert-hash sha256:4559699a31c808c75dcf49e99cbd92e881ff7751f35f321ebda79d1d83fcaa13
  ```
3. На 1 VM проверяем подключение nodes, а так же корректный запуск остальных pod
```
kubectl get nodes
kubectl get pod -A -o wide
```
4.


