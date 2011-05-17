# Validação na camada de negócio

Vamos validar uma classe que contem um documento como atributo. Veja o exemplo abaixo:
```java
public class Pessoa {  
    private String cpf;  
    // getters e setters omitdos  
}  
```
Seria interessante se nos fosse fornecida uma classe que validasse este bean.

Com o Bean Validation isto é possível. Para isso, devemos indicar quais são as regras que validam esse bean. No nosso caso, devemos indicar que um é bean válido se seu atributo cpf é válido.

Vamos inserir a anotação de validação sobre o atributo desejado.
```java
@CPF 
private String cpf;
```

Esta anotação está disponíveis na distribuição do Caelum-Stella Hibernate Validator, portanto não esqueça de incluir no seu classpath as bibliotecas necessárias.

Agora, utilizando a API de validação do Bean Validation, podemos validar instâncias do nosso bean. Veja como é simples.
```java
Pessoa alguem = new Pessoa(); 
alguem.setCpf("XXXXXXXXXXX"); 

Validator validator = Validation.buildDefaultValidatorFactory().getValidator();

Set<ConstraintViolation<Pessoa>> violations = validator.validate(alguem);

if(!violations.isEmpty())
    System.out.println("Existem documentos inválidos")
```
Após a execução deste programa a saída do console apresentará a mensagem informando que existem documentos inválidos.

Veja agora como podemos indentificar os erros de validação que ocorreram.

```java
for (ConstraintViolation<Pessoa> violation : violations) {
    System.out.println(violation.getMessage());
}
```
O programa irá imprimir as mensagens configuradas no arquivo ValidatorMessages.properties.