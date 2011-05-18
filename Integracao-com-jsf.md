# Integrando com a camada de visualização

## Integração com JSF 2

JSF 2 possui integração nativa com Bean Validation. Em ambientes onde exista uma implementação de bean validation no classpath, o JSF irá validar todos os beans referenciados pelos inputs nas páginas. Como as anotações do Stella são compatíveis com Bean Validations, não é necessário realizar nenhuma nova configuração para que sua validação seja utilizada na camada de visualização.

Vaje um exemplo de validação

```java
public class Pessoa {  
    @CPF
    private String cpf;  
    // getters e setters omitdos  
}  

@ManagedBean
@RequestScoped
public class RegistraFuncionarioBean {
    @CNPJ
    private String cnpj;
   
    private Pessoa funcionario;
    // getters e setters omitdos  
...
}
```
Neste exemplo, tanto o CPF quanto o CNPJ serão validados pelo JSF

## Integração com JSF 1x
Para saber mais sobre JSF e outras formas de integração com o Stella, acesse a página do [[Stella Faces|stella-faces]].
