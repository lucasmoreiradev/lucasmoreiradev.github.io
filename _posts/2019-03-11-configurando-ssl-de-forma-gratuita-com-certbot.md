---
layout: post
title: "Configurando SSL de forma gratuita com certbot"
date: 2019-03-11 01:57:49
image: '/assets/img/certificado-ssl-certbot/certbot.png'
description: 'Como configurar um certificado SSL de forma gratuita com certbot.'
main-class: 'devops'
color: '#D6BA32'
tags: 
- certbot
- devops
- nginx
- ssl
- https
categories:
twitter_text: "Configurando SSL de forma gratuita com certbot"
introduction: "Quer aprender a configurar SSL pro seu sistema de forma gratuita com o certbot? Este post pode te ajudar!"
---

## Introdução

Fala, galera! Nesse post vou ensinar a configurar um certificado SSL de forma gratuita para qualquer sistema que use o **nginx** como proxy reverso. O certificado SSL gerado tem uma data de validade menor, mas é possível fazer a renovação de forma automática, que também vou mostrar aqui.

## Instalação do nginx

Instale o nginx conforme o seu sistema operacional, no meu caso fiz a instalação é um CentOS7/RHEL - ec2 da aws - com o gerenciador de pacotes **yum**: 

`sudo yum install nginx`

## Configurando o nginx para fazer proxy no sistema

Basta criar um arquivo `custom.conf`, dentro da pasta `conf.d.`, com o seguinte conteúdo: 

`vim /etc/nginx/conf.d/custom.conf`:

```
server {
  server_name seudominio.com.br;
  location / {
    proxy_pass http://localhost:9000;
  }
}
```

obs: coloque a porta que o seu site/sistema está rodando. No meu caso é a `9000`. 

Após isso basta reiniciar o serviço do nginx (`service nginx restart`) e o sistema já estará funcionado através do `seudomínio.com.br` **sem SSL**. O certbot entra em ação agora para ativar o SSL e fazer funcionar o **https** de forma fácil. Vamos lá!

## Instalando e configurando o certbot

Existem diversas formas de se instalar o certbot. Pode-se utilizar o gerenciador de pacotes do sistema operacional ou baixar o certbot direto do site deles. Eu prefiro a segunda opção simplesmente por poder fazer da mesma forma em diferentes distros do Linux. 

Faça o download do certbot em alguma pasta com o seguinte comando: 

`wget https://dl.eff.org/certbot-auto`

Mova o arquivo baixado para a basta `/usr/bin`: 

`mv certbot-auto /usr/bin` e dê permissão para que o arquivo possa fazer alterações na máquina: `sudo chown root /usr/bin/certbot-auto` e `sudo chmod 0755 /usr/bin/certbot-auto`.

Pronto! Já está devidamente instalado! Agora é só rodar o comando para que ele possa criar o certificado ssl de forma automática no **nginx**. O comando é: 

`certbot-auto --nginx`

Após rodar este comando basta preencher o que o CLI dele pedir, escolher as opções que você preferir e... TCHANANN 🎉 o seu sistema já estará funcionando com SSL! =) 

Como dito anteriormente, o tempo de validade do certificado SSL gerado é um pouco curto - cerca de 3 meses. Então é legal configurar para que o próprio sistema operacional faça a renovação de forma automática. Para isso, vamos o usar o `crobtab` do Linux.

Basta digitar: `crontab -e` e depois preencher o arquivo com:  `30 2 * * Sun certbot-auto renew`.

## Conclusão

Bom... espero que vocês também consigam colocar https em seus sistemas e sites de forma fácil com o certbot. Valeu! =)
