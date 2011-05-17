Vamos validar um documento de CPF utilzando o Stella Core.

```java
String cpf = "222.888.444-52";
```

O Stella forncede uma classe que valida este documento.

```java
CPFValidator validator = new CPFValidator();
```

Agora, veja como é simples realizar a validação do documento.
```java
    try {  
        // lógica de negócio ...  
        validator.assertValid(cpf);  
        // continuação da lógica de negócio ...  
    } catch (InvalidStateException e) {  // exception lançada quando o documento é inválido
        System.out.println(e.getInvalidMessages());  
    }
```

Após a execução deste programa a saída do console apresentará a seguinte linha:
**[CPFError : INVALID CHECK DIGITS]**

Ao chamar o método `assertValid()` este lançará uma exceção (do tipo unchecked): `InvalidStateException`.

Podemos capturar as messagens de erro utilizando o método `invalidMessagesFor()`. Este método **não** lança a `InvalidStateException`.
```java
// lógica de negócio ...  
List<ValidationMessage> validationMessages = validator.invalidMessagesFor(cpf);  
System.out.println(validationMessages.getInvalidMessages());  
// continuação da lógica de negócio ...  
```

Após a execução deste programa a saída do console será a mesma: **[CPFError : INVALID CHECK DIGITS]**.

##Lista de todos os validadores
Cada validador pode produzir diversos erros, com diferentes mensagens. A lista abaixo é uma referência de todos os validadores disponiveis, e de todas as mensagens de erro que podem produzir.  

* **CPFValidator**
    * CPFError.INVALID_DIGITS
    * CPFError.INVALID_FORMAT
    * CPFError.INVALID_CHECK_DIGITS
    * CPFError.REPEATED_DIGITS
* **CNPJValidator**
    * CNPJError.INVALID_DIGITS
    * CNPJError.INVALID_FORMAT
    * CNPJError.INVALID_CHECK_DIGITS
* **NITValidator**
    * NITError.INVALID_DIGITS
    * NITError.INVALID_FORMAT
    * NITError.INVALID_CHECK_DIGITS
* **InscricaoEstatudalDe...Validator (Para todos os estados)**
    * IEError.INVALID_DIGITS
    * IEError.INVALID_FORMAT
    * IEError.INVALID_CHECK_DIGITS
* **TituloEleitoralValidator**
    * TituloEleitoralError.INVALID_DIGITS
    * TituloEleitoralError.INVALID_CHECK_DIGITS
    * TituloEleitoralError.INVALID_CODIGO_DE_ESTADO

