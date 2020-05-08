---
layout: post
comments: true
title: "Gerando classes Java a partir de arquivos WSDL e XSD"
date: 2020-05-07 22:15:56
image: '/assets/img/'
description: 'Como criar classes Java para consumir WebServices SOAP de forma fácil.'
main-class: 'dev'
color:
tags:
- Java
- SOAP
- WebServices
- JAXB
categories:
twitter_text: 'Como criar classes Java para consumir WebServices SOAP de forma fácil.'
introduction: 'Como criar classes Java para consumir WebServices SOAP de forma fácil.'
---

Fala, pessoal! Beleza? Nesse post decidi escrever um pouco mais sobre algumas ferramentas que facilitam o consumo de WebServices SOAP com Java.


Para consumir um WS SOAP precisamos seguir um contrato de serviço, que é o WSDL (Web Description Language). Através deste contrato, precisamos enviar arquivos XML, que podem ser expressos via arquivos XSD (XML Schema Definition).

![Imagem de WSDL](https://miro.medium.com/max/1000/1*M7V2X4LQ2HHadCvm66Si3g.jpeg)

Quem está acostumado a lidar apenas com APIs REST e nunca trabalhou com SOAP, pode achar bem mais complicado. E na verdade é, mas existem algumas ferramentes que podem vacilitar bastaaante no nosso trabalho!

## Ferramentas

Com o Axis2, por exemplo, podemos pegar um arquivo *wsdl* e gerar as classes de consumo Java automaticamente. Basta fazer o seguinte:

### Axis

1	- Baixar o axis2: [http://axis.apache.org/axis2/java/core/download.html](http://axis.apache.org/axis2/java/core/download.html) - Escolher Binary distribution.
2 - Entrar na pasta do **axis/bin**  e executar o seguinte comando: 

`wsdl2java.bat -uri <seu.wsdl> -o <output_dir> -noBuildXML  -p br.com.projeto`

Onde `-uri` é onde está localizado seu arquivo `wsdl`, `-o` é o diretório onde as classes serão geradas, `-noBuildXML` serve para não gerar o arquivo `output.xml` no diretório onde serão geradas as classes, e `-p` é o pacote em que as classes serão geradas.

### JAX-B

Pronto! Com isso já temos as classes de consumo do WebService prontinhas! Mas e os XMLs que serão enviados? Como podemos gerá-los também facilmente? É aí que entra o JAX-B:

[JAXB](https://jcp.org/aboutJava/communityprocess/mrel/jsr222/)  é parte da JDK, é um dos frameworks mais utilizados para processar documentos XML. Ele possui uma pequena ferramenta de linha de comando chamado [xjc](http://docs.oracle.com/javase/6/docs/technotes/tools/share/xjc.html). Ou seja, se você tem a JDK instalada, não precisa instalar mais nada. Basta rodar o seguinte comando:

`xjc -d <dir> -p <pkg> seu_arquivo.xsd`

Onde `-d` é o diretório do seu projeto e `-p` é o pacote que você deseja colocar as classes geradas.

Pronto! Agora é só popular as classes geradas pelo JAXB e consumir o WebService através das classes geradas pelo Axis2. 

ps: É importante ver a documentação de consumo do WebService, pois podem existir mais alguns passos, como autenticação e protocolo de segurança. :)

## Conclusão

Mesmo a grande maioria dos serviços hoje serem disponibilizados através de APIs REST, também é importante saber como funcionam os serviços disponibilizados em SOAP, porque ainda existem muitos sistemas construidos dessa forma. E quem for dar manutenção em legado com certeza vai ter que utilizar. 

Um grande exemplo que eu posso dar são as Notas Fiscais Eletrônicas da Secretaria de Fazenda. Tudo é feito com SOAP e a gente que se vire para consumir. 😬

E vocês, já tiveram que lidar com WebServices SOAP? 

Deixem nos comentários!