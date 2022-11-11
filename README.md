Используемое "железо":
Raspberry Pi 4B (в моем случае на 4gb ОЗУ)
Корпус Argon ONE M.2 (возможно приобрести сразу в комплекте с SSD)

Кабель питания Ugreen USB C - USB C
Кабель USB-USB
Блок питания Ugreen 

__________________________________________________________________

# Полезное ПО:
Ссылка на страницу загрузки утилиты для записи образа на флешку - https://www.raspberrypi.com/software/
Ссылка на страницу загрузки SSH клиента Putty - https://www.putty.org/
__________________________________________________________________

Работа с консолью:

1. Переходим в режим root
sudo -s

2. Обновляем список пакетов и сами пакеты
apt update
apt upgrade -y

3. Обновляем прошивку
rpi-update

4. Устанавливаем необходимые для работы пакеты
apt-get install -y jq wget curl udisks2 apparmor-utils libglib2.0-bin network-manager dbus systemd-journal-remote

5. Запускаем Network Manager
systemctl start NetworkManager
systemctl enable NetworkManager

6. Приложение для настройки -
sudo raspi-config

Переходим по пути:
5 Localisation Options / I1 Change Locale - ищем и выбираем локаль ru_RU.UTF-8 UTF-8
5 Localisation Options / I2 Change Timezone - ищем и выбираем часовой пояс

7. Дополнительные настройки
nano /boot/cmdline.txt
В конец файла вставляем systemd.unified_cgroup_hierarchy=false lsm=apparmor
(Ctrl X - для выхода, Y для сохранения изменений)

8. Устанавливаем скрипт управления кулером (для тех кто использует корпус Argon ONE M.2)
curl https://download.argon40.com/argon1.sh | bash
Настраиваем пороги включения кулера
argonone-config

9. Перезагружаемся
sudo reboot

10. Устанавливаем docker
sudo curl -fsSL get.docker.com | sh

11. Добавляем пользователя в группу docker
sudo gpasswd -a $USER docker
newgrp docker

12. Установка OS-Agent (Ссылка на актуальные версии - https://github.com/home-assistant/os-agent/releases/tag/1.4.1)
Загружаем (номер меняем на актуальный) 
wget https://github.com/home-assistant/os-agent/releases/download/1.4.1/os-agent_1.4.1_linux_aarch64.deb 
Устанавливаем 
sudo dpkg -i os-agent_1.4.1_linux_aarch64.deb

13. Устанавливаем Home Assisistant Supervised
Загружаем
wget https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb
Устанавливаем 
sudo dpkg -i homeassistant-supervised.deb

__________________________________________________________________

Ссылки:
Ссылка на веб интерфейс Home Assistant - http://your_IP_adress:8123
Ссылка на страницу с информацией о системе - http://your_IP_adress:8123/hassio/system
__________________________________________________________________

Для удобства на боковую панель можно вывести кнопку, для этого в configuration.yaml добавим:

panel_custom:
  - name: server_state
    sidebar_title: 'Система'
    sidebar_icon: mdi:server
    js_url: /api/hassio/app/entrypoint.js
    url_path: 'hassio/system'
    embed_iframe: true
    require_admin: true
    config:
      ingress: core_configurator
