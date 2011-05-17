# Mensagens de erro

Você pode sobrescrever as mensagens de erro criando um *ValidatorMessages.properties* e inserir nele os campos desejados. Se o Bean Validation não conseguir resolver uma chave a partir do seu *resourceBundle* ou do *ValidatorMessages*, ele retornará o valor embutido padrão.

Exemplo de arquivo de validação:

    # ValidatorMessages.properties
    cpf_invalid  = CPF inválido 
    cnpj_invalid = CNPJ inválido

Opicionalmente, você pode definir as mensagens diretamente nas anotações:

```java
public class Pessoa {
    @CPF(message="CPF inválido")
    private String cpf;  
    // getters e setters omitdos  
}  
```
