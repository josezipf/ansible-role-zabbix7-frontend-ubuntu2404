# Role Ansible: zabbix7_frontend_ubuntu2404

Esta role instala e configura o **Zabbix Frontend 7.0 LTS** no **Ubuntu 24.04**, utilizando **Apache** e **PHP 8.3** como base.

## 📦 O que essa role faz

- Instala os pacotes necessários do frontend do Zabbix
- Configura o arquivo `zabbix.conf.php` via template Jinja2
- Habilita e inicia o serviço `apache2`
- Usa MySQL como backend

## 📁 Estrutura da Role

```
zabbix7_frontend_ubuntu2404/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   └── zabbix.conf.php.j2
├── tests/
│   └── test.yml
├── vars/
│   └── main.yml
└── README.md
```

## ⚙️ Variáveis padrão (`defaults/main.yml`)

```yaml
mysql_database: zabbix
mysql_zabbix_user: zabbix
mysql_zabbix_password: SenhaZabbix
```

Essas variáveis podem ser sobrescritas conforme necessidade no playbook ou inventário.

## 📦 Pacotes instalados (`vars/main.yml`)

```yaml
zabbix_frontend_packages:
  - zabbix-frontend-php
  - zabbix-apache-conf
  - php8.3
  - php8.3-mysql
  - php8.3-gd
  - php8.3-bcmath
  - php8.3-mbstring
  - php8.3-xml
  - php8.3-ldap

php_timezone: "America/Sao_Paulo"
```

## 🧾 Template do frontend (`templates/zabbix.conf.php.j2`)

```php
<?php
global $DB;

$DB['TYPE']     = 'MYSQL';
$DB['SERVER']   = 'localhost';
$DB['PORT']     = '3306';
$DB['DATABASE'] = '{{ mysql_database }}';
$DB['USER']     = '{{ mysql_zabbix_user }}';
$DB['PASSWORD'] = '{{ mysql_zabbix_password }}';

$DB['SCHEMA'] = '';

$ZBX_SERVER      = 'localhost';
$ZBX_SERVER_PORT = '10051';
$ZBX_SERVER_NAME = 'Zabbix Server';

$IMAGE_FORMAT_DEFAULT = IMAGE_FORMAT_PNG;
?>
```

## 🔁 Handler (`handlers/main.yml`)

```yaml
- name: Restart Apache
  ansible.builtin.service:
    name: apache2
    state: restarted
  become: true
```

## ✅ Teste local (`tests/test.yml`)

```yaml
- name: Teste da role zabbix7_frontend_ubuntu2404
  hosts: localhost
  become: true
  roles:
    - role: zabbix7_frontend_ubuntu2404
```

## 🧪 Teste via linha de comando

```bash
ansible-playbook roles/install_monitoring/zabbix7_frontend_ubuntu2404/tests/test.yml
```

## 📋 Requisitos

- Ubuntu 24.04 LTS
- Ansible 2.10 ou superior
- MySQL funcionando (backend)
- Apache2 e PHP 8.3 disponíveis

## 🏷️ Tags Ansible Galaxy

- `zabbix`
- `frontend`
- `apache`
- `php`
- `ubuntu`
- `monitoring`

## 🧑 Autor

**José Zipf**  
Licença: MIT  
[https://nototi.com.br](https://nototi.com.br)
