# ansibleDatadogAgent


## setting datadog_api_key
$ vi ./vars/data-dog-agent-basic.yml
```
- datadog_api_key: "{🐱 datadog_api_keyを入力してください 🐱}"
+ datadog_api_key: "xxxxxxx"
```
  
## デプロイ例
### laravelアプリ
  
# ansible-playbook /etc/ansibleDatadogAgent/ubuntu18-laravel-nginx-php-fpm.yml --extra-vars "service=ubuntu18-laravel-nginx-php-fpm env=develop project=sampleApp " -v -i /etc/ansibleDatadogAgent/hosts/servers
  
### ジョブワーカー
# ansible-playbook /etc/ansibleDatadogAgent/ubuntu18-laravel-nginx-php-fpm-supervisord.yml --extra-vars "service=ubuntu18-laravel-nginx-php-fpm-supervisord env=develop project=sampleApp " -v -i /etc/ansibleDatadogAgent/hosts/servers
  
* --extra-vars
  * service … サービス名。Datadog上での表示Hostnameにもなる
  * env … develop| staging | production
  * project … プロジェクト名。/var/www/{プロジェクト名}に対応するようにする