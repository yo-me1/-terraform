Плавающему IP-адресу;
Смотрим туннельные соединения;
Проверяем связность

Создаём playbook который будет реализовывать:WebADM так, чтобы она могла подключаться по SSH с использованием пользователя altlinux и пароля «P@ssw0rd» к инстансам WEB1 и WEB2 с помощью VPN туннеля
vim ansible/ssh_playbook.yml

Помещаем в него следующее содержимое:
---
- hosts: WEB1 WEB2
become: true

tasks:
- name: Setting 'sshd_config' file
ansible.builtin.lineinfile:
line: "{{ item }}"
path: /etc/openssh/sshd_config
state: present
with_items:
- "PasswordAuthentication no"
- "Match address 10.20.30.0/29"
- " PasswordAuthentication yes"

- name: Restarted sshd
ansible.builtin.systemd:
name: sshd
state: restarted

Запускаем playbook-сценарий для настройки SSH по VPN-туннелю:
ansible-playbook -i ansible/inventory ansible/ssh_playbook.yml

apt-get install tree

tree
