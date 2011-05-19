# Stella Faces

É o modulo do Caelum Stella que tem como objetivo fornecer uma série de componentes do Java Server Faces, facilitando a vida dos desenvolvedores brasileiros que adotam esta tecnologia.

Uma das características mais fortes do Caelum Stella-Faces é a validação com agilidade e simplicidade. Veja os exemplos de validação com JSP, XHTML (do Facelets) e repare a simplicidade de uso.

Os componentes são compatíveis com qualquer implementação do JSF e podem ser usados tando com JSP quanto com  Facelets.

## Configurando o ambiente

Para usar o Stella Faces é necessário fazer o download de alguma implementação do JSF.

A implementação Sun está disponível em [[https://javaserverfaces.dev.java.net]] .

A implementação do grupo Apache (MyFaces) está disponível em [[http://myfaces.apache.org]].

Lista de jars necessários no classpath:

* caelum-stella-faces 2x
* caelum-stella-core 2x
* jstl-1.2
* jsf-api-1.x ou 2.x
* jsf-impl-1.x ou 2.x
* jsf-facelets-1.1.x (opcional, somente para usar Facelets em JSF 1.x)

## Arquivos de configuração

Para configurar as mensagens de erro utilize o recurso de i18n fornecido pelo Java Server Faces e deixe as suas mensagens no seu arquivo de mensagens (.properties). Se não estiver familiarizado com este tipo de arquivo, leia o [[tutorial do J2EE|http://download.oracle.com/javaee/6/tutorial/doc/bnaxu.html]].

As chaves das mensagens neste arquivo devem seguir a convenção do `ResourceBundleMessageProducer`. 

## Utilizando o Stella Faces
Para saber mais sobre o Caelum Stella faces, consulte os links abaixo:

* [[Validando com Stella-Faces|faces-1x]]
