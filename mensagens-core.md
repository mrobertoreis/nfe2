#Produzindo mensagens de validação

## Motivação
Ao identificar um documento inválido, na camada de apresentação, devemos notificar o usuário com um mensagem. Dependendo do contexto, podemos sentir a necessidade de apresentar mensagens personalizadas, como por exemplo em diferentes idiomas ou simplesmente seguir um padrão de mensagem. Dessa necessidade surgiu a interface `br.com.caelum.stella.MessageProducer`. As classes que a implementam são responsáveis por fornecer uma mensagem de validação para cada erro.

Neste módulo estão disponíveis a `ResourceBundleMessageProducer` e a `SimpleMessageProducer`.

##Lógica de geração da chave de erro
Os erros de validação são representados por classes que implementam a interface `br.com.caelum.stella.validation.InvalidValue`. No Caelum Stella, estes errors costumam ser **enums**, que implementam esta interface.

Com base nesta interface, o padrão para as chaves de validação são formadas por nomes: o da classe e o da enum.

Suponha que deseja-se econtrar uma mensagem de erro para a classe `CPFError`, cuja instancia (dessa enum) representa um erro do tipo **INVALID_FORMAT**. A chave gerada é a composição desses dois nomes (em letras minúsculas) separados por um ponto. Ou seja, *"cpferror.invalid_format"*.

##SimpleMessageProducer
O `SimpleMessageProducer` é um produtor de mensagens muito simples. Ele também utiliza o nome da classe e da enum para gerar a mensagem de validação. Ao invés de gerar uma chave para busca em algum arquivo de mensagens, este produtor cria a mensagem sem consultar nenhum outro recurso. A mensagem gerada é semelhante a chave descrita anteriormente, no entanto troca-se o primeiro ponto `"."` por `" : "` e todos underscores `"_"` por espaços em branco `" "`.

A mensagem gerada pelo exemplo anterior seria : *"cpferror : invalid format"*. Assim, com pouca configuração, temos uma mensagem mais legível.

#Utilizando Resource Bundle

Vamos validar um documento de CNPJ utilzando o Stella Core e buscaremos as mensagens de erro em um arquivo.
```java
String cnpj = "26.637.142/0001-48";
```
O `MessageProducer` que instanciaremos irá buscar as mensagens em um arquivo. Para isso passamos o `ResourceBundle` como argumento do construtor.

```java
ResourceBundle resourceBundle = ResourceBundle.getBundle("StellaValidationMessages",new Locale("pt","BR"));  
MessageProducer messageProducer = new ResourceBundleMessageProducer(resourceBundle);  
boolean isFormatted = true;  
CNPJValidator validator = new CNPJValidator(messageProducer,isFormatted);  
```
Portanto, devemos ter o arquivo StellaValidationMessages_pt_BR.properties no classpath: 

    # StellaValidationMessages_pt_BR.properties    
    # Erros de CPF  
    cpferror.invalid_digits  = CPF inválido  
    cpferror.invalid_check_digits  = CPF inválido  
    cpferror.invalid_format  = CPF inválido  
    # Erros de CNPJ  
    cnpjerror.invalid_digits = CNPJ inválido  
    cnpjerror.invalid_check_digits = CNPJ inválido : Dígitos verificadores incorretos  
    cnpjerror.invalid_format = CNPJ inválido   

Agora, veja como é simples realizar a validação do documento.
```java
try {  
    // lógica de negócio ...  
    validator.assertValid(cnpj);  
    // continuação da lógica de negócio ...  
} catch (InvalidStateException e) {  
    for (ValidationMessage message : e.getInvalidMessages()) {  
    System.out.println(message.getMessage());  
}
```
Após a execução deste programa a saída do console apresentará a seguinte linha: 
**[CNPJ inválido : Dígitos verificadores incorretos]**

Veja abaixo código completo:

```java
package com.yourcompany.example;  

import java.util.Locale;  
import java.util.ResourceBundle;  

import br.com.caelum.stella.MessageProducer;  
import br.com.caelum.stella.ResourceBundleMessageProducer;  
import br.com.caelum.stella.ValidationMessage;  
import br.com.caelum.stella.validation.CNPJValidator;  
import br.com.caelum.stella.validation.InvalidStateException;  

public class CoreExample {
    public static void main(String[] args) {  
        String cnpj = "26.637.142/0001-48";  
        ResourceBundle resourceBundle = ResourceBundle.getBundle("StellaValidationMessages",
                                                                 new Locale("pt","BR"));  
        MessageProducer messageProducer = new ResourceBundleMessageProducer(resourceBundle);  
        boolean isFormatted = true;  
        CNPJValidator validator = new CNPJValidator(messageProducer, isFormatted);  

        try {  
            // lógica de negócio ...  
            validator.assertValid(cnpj);  
            // continuação da lógica de negócio ...  
        } catch (InvalidStateException e) {  
            for (ValidationMessage message : e.getInvalidMessages()) {  
                System.out.println(message.getMessage());  
            }  
        }  
    }  
}
```