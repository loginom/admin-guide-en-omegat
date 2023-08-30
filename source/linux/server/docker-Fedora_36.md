# Предварительная установка ПО (Podman)

Перед инсталляцией Loginom необходимо предварительно установить и настроить ПО.

Протестировано на **Fedora 36**.

Нужно выполнить пункты:

1. Устанавить необходимые пакеты.

   ```
   sudo dnf update && sudo dnf install -y podman podman-compose
   ```

2. Разрешить Podman-у использовать в непривилегированном режиме порты, начиная с 80.

   ```
   grep -q "net.ipv4.ip_unprivileged_port_start=80" "/etc/sysctl.conf" || echo "net.ipv4.ip_unprivileged_port_start=80" | sudo tee -a /etc/sysctl.conf

   sudo sysctl -w net.ipv4.ip_unprivileged_port_start=80
   ```

3. Разрешить пользовательской сессии оставаться активной после выхода.
   ```
   sudo loginctl enable-linger
   ```

---
## Настройка работы контейнеров Loginom при наличии SELinux

В ОС Linux семейства RHEL, куда относятся, например, RedOS и Fedora, работает модуль безопасности ядра. Для нормальной работы системы контейнеризации возможны варианты:

1. Настроить разрешения SELinux на монтирование томов. Необходимо в файле конфигурации, располагаемом по умолчанию `${HOME}/loginom/docker-compose.yml`, произвести некоторые изменения. Во все разделы монтирования томов, кроме монтирования файла сокета `run/systemd/journal/socket`, необходимо добавить опцию безопасности `:Z`, например:

    ```
    - "${HTTP_DIR}/server.json:/usr/local/apache2/htdocs/client/server.json:Z"
    ```

2. Отключить SELinux.

   ```
   sudo setenforce 0

   sudo sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/selinux/config
   ```