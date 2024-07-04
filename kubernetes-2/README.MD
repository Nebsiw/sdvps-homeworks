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
    --apiserver-advertise-address=192.168.123.5 \
    --pod-network-cidr 172.16.0.0/16
    ```



