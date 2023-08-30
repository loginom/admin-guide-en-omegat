# Предварительная установка ПО (Podman)

Перед инсталляцией Loginom необходимо предварительно установить и настроить ПО.

Протестировано на **OpenSUSE 15.4**.

Нужно выполнить пункты:

1. Установить необходимые пакеты.

   ```
   sudo zypper install -y podman podman-compose cni-plugin-dnsname

   ```

2. Разрешить процессу dnsmasq открывать необходимые файлы.

   ```
   echo "/run/user/*/containers/cni/dnsname/*/dnsmasq.conf r, /run/user/*/containers/cni/dnsname/*/addnhosts r, /run/user/*/containers/cni/dnsname/*/pidfile rw," | sudo tee -a "/etc/apparmor.d/local/usr.sbin.dnsmasq"

   sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.dnsmasq
   ```

3. Разрешить Podman-у использовать в непривилегированном режиме порты, начиная с 80.

   ```
   grep -q "net.ipv4.ip_unprivileged_port_start=80" "/etc/sysctl.conf" || echo "net.ipv4.ip_unprivileged_port_start=80" | sudo tee -a /etc/sysctl.conf

   sudo sysctl -w net.ipv4.ip_unprivileged_port_start=80
   ```

4. Разрешить пользовательской сессии оставаться активной после выхода.

   ```
   sudo loginctl enable-linger
   ```
