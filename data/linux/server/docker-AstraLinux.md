# Предварительная установка ПО (Docker)

Перед инсталляцией Loginom необходимо предварительно установить и настроить ПО.

Протестировано на **Astra Linux 1.7**.

Нужно выполнить пункты:

1. Обновить до версии 1.7.3.

   Для этого добавить репозитории версии 1.7.3, обновить пакеты и саму систему, в финале перезагрузиться.

   ```
   sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates

   echo "deb https://dl.astralinux.ru/astra/frozen/1.7_x86-64/1.7.3/repository-main/ 1.7_x86-64 main contrib non-free" | sudo tee -a "/etc/apt/sources.list"

   echo "deb https://dl.astralinux.ru/astra/frozen/1.7_x86-64/1.7.3/repository-update/ 1.7_x86-64 main contrib non-free" | sudo tee -a "/etc/apt/sources.list"

   echo "deb https://dl.astralinux.ru/astra/stable/1.7_x86-64/repository-extended/ 1.7_x86-64 main contrib non-free" | sudo tee -a "/etc/apt/sources.list"

   sudo sed -i -E "s~^.*\s+cdrom:.*~#\0~" "/etc/apt/sources.list"

   sudo apt-get update && sudo apt-get upgrade

   sudo astra-update -A -r

   sudo reboot
   ```

2. Установить необходимые пакеты.

   ```
   sudo apt-get install -y docker docker-compose
   ```
