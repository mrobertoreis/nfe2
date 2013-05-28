# Mensagens de erro

Você pode sobrescrever as mensagens de erro criando um *ValidationMessages.properties* e inserir nele os campos desejados. Se o Bean Validation não conseguir resolver uma chave a partir do seu *resourceBundle* ou do *ValidationMessages*, ele retornará o valor embutido padrão.

Exemplo de arquivo de validação:

```properties
#ValidatorMessages.properties
br.com.caelum.stella.bean.validation.CPF.message= CPF inválido 
br.com.caelum.stella.bean.validation.CNPJ.message= CNPJ inválido
```

Opcionalmente, você pode definir as mensagens diretamente nas anotações:

```java
public class Pessoa {
    @CPF(message="CPF inválido")
    private String cpf;  
    // getters e setters omitdos  
}  
```