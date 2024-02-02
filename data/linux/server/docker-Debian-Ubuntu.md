# Предварительная установка ПО (Podman)

Перед инсталляцией Loginom необходимо предварительно установить и настроить ПО.

Протестировано на **Debian 11.5** и **Ubuntu 22.04**.

Нужно выполнить пункты:

1. Установить необходимые пакеты.

   ```
   sudo apt-get update && sudo apt-get install -y podman

   sudo apt-get install -y pip && sudo pip3 install podman-compose

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
