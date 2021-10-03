# ansibleDatadogAgent


## allow deploy taget server
  
### ターゲットサーバ側で作業
  
ansibleサーバからの接続許可ユーザを作成
```
$ useradd -m -d /home/ansible_deployer/ -s /bin/bash -G sudo ansible_deployer
$ touch /etc/sudoers.d/ansible_deployer
$ echo "ansible_deployer ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible_deployer
$ sudo su - ansible_deployer
$ mkdir /home/ansible_deployer/.ssh
$ echo "{SSH公開鍵}" > /home/ansible_deployer/.ssh/authorized_keys
$ chmod 700 /home/ansible_deployer/.ssh
$ chmod 600 /home/ansible_deployer/.ssh/authorized_keys
```

### Ansibleサーバ側作業

Ansibleのインストール
```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible

$ ansible --version

ansible [core 2.11.5]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.8/dist-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Jun  2 2021, 10:49:15) [GCC 9.4.0]
  jinja version = 2.10.1
  libyaml = True
```


## clone this app
```
# cd /etc/
# git clone https://github.com/yuukanehiro/ansibleDatadogAgent.git
```

## install datadog-galaxy
  
```
$ sudo ansible-galaxy install datadog.datadog
```
  
## generate datadog_api_key
  
https://app.datadoghq.com/account/settings#api

## setting datadog_api_key
  
```
$ vi ./vars/data-dog-agent-basic.yml

- datadog_api_key: "{🐱 datadog_api_keyを入力してください 🐱}"
+ datadog_api_key: "xxxxxxx"
```
  
## デプロイ例
  
### Laravel App
  
```
# ansible-playbook /etc/ansibleDatadogAgent/ubuntu18-laravel-nginx-php-fpm.yml --extra-vars "service=ubuntu18-laravel-nginx-php-fpm env=develop project=sampleApp " -v -i /etc/ansibleDatadogAgent/hosts/servers
```
  
### Laravel job-worker
  
```
# ansible-playbook /etc/ansibleDatadogAgent/ubuntu18-laravel-nginx-php-fpm-supervisord.yml --extra-vars "service=ubuntu18-laravel-nginx-php-fpm-supervisord env=develop project=sampleApp " -v -i /etc/ansibleDatadogAgent/hosts/servers
```
  
* --extra-vars
  * service … サービス名。Datadog上での表示Hostnameにもなる
  * env … develop| staging | production
  * project … プロジェクト名。/var/www/{プロジェクト名}に対応するようにする