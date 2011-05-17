# Stella Bean Validation

O Caelum Stella Bean Validation é uma API que fornece anotações para validação de documentos brasileiros. As anotações são uma forma muito conveniente e elegante para descrever as restrições invariantes no modelo de domínio.

As anotações fornecidas são compatíveis com a especificação de Bean Validation. Ao utilizá-lo, nos beneficiamos do princípio **DRY** (**D**on't **R**epeat **Y**ourself). Isto significa que podemos expressar as restrições de domínio apenas uma vez e garantir um domínio coeso em vários níveis do sistema.Para saber mais, leia o [guia de referência do Bean Validation](http://download.oracle.com/javaee/6/tutorial/doc/gircz.html)

## Configurando o Ambiente

Lista de jars necessários no classpath:

* caelum-stella-core 2x
* validation-api 1.x 
* mirror 1.5

Também é necessário ter uma implementação da api de validations, por exemplo o hibernate-validator 4x.

## Arquivos de configuração

Para configurar as mensagens de erro utilize o arquivo *ValidatorMessages.properties*.

## Utilizando o Stella Bean Validation
Para saber mais sobre o Caelum Stella Bean Validation, consulte os links abaixo:

* [[Anotando seu modelo|anotando-o-modelo]]
* [[Persistindo o modelo|persistindo-o-modelo]]
* [[Validando na camada de negócios|camada-de-negocios]]
* [[Integração com JSF|integracao-com-jsf]]
* [[Configurando mensagens|mensagens-bean-validation]]