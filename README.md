# Zabbix em Docker Compose com SSL

O arquivo docker-compose.yml que disponibilizamos em nosso Github, foi configurado de forma que o Docker crie 4 containers: zabbix-server, zabbix-frontend, grafana e mysql. Foram utilizadas as imagens oficiais do Zabbix, do Grafana e do MySQL. Os links para consulta estão no final deste artigo. 

Ao executar o comando docker-compose up, o Docker irá subir de forma automática os containers do Zabbix, do Grafana e do MySQL. Além disso, o Zabbix já estará conectado ao banco de dados MySQL e o Grafana já estará com o plugin do Zabbix instalado.

Executar o seguinte comando na pasta que esta localizado o arquivo docker-compose.yml:
 - Gerar o certificado SSL
 
yum install mod_ssl

mkdir -p /etc/httpd/ssl/private

chmod 700 /etc/httpd/ssl/private

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/private/apache-selfsigned.key -out /etc/httpd/ssl/apache-selfsigned.crt

- Ajusta para o Zabbix ler os arquivos SSL

cd /etc/httpd/ssl/

mv apache-selfsigned.crt ssl.crt

mv private/apache-selfsigned.key ./

mv apache-selfsigned.key ssl.key

- Adicionar a pasta dos certificados no arquivo do docker compose no zabbix-frontend

volumes:
      - '/etc/httpd/ssl/:/etc/ssl/apache2'

docker-compose up -d



Fonte:
https://www.zabbix.com/documentation/3.4/manual/installation/requirements/best_practices
