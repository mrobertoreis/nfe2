# Integração com VRaptor

A integração do Vraptor com os validadores do Stella é muito simples através do Stella-Bean-Validation:

```java
//Bean anotado com os validadores do Stella
public class Usuario {
    @CPF
    private String cpf;
    //getters e setters omitidos
}

//Controller que realiza a validação
@Resource
public class UsuarioController {
    //Validator do VRaptor	
	private Validator validator;
	
	public UsuarioController(Validator validator) {
		this.validator = validator;
	}
    //exibe o formuário de cadastro de usuário
    public void formulario() {}
	
    public void adiciona(Usuario usuario) {
    	validator.validate(usuario);
    	validator.onErrorUsePageOf(UsuarioController.class).formulario();
    	
    	//código para adicionar o usuário
    	...
    }
    ...
}
```
Se houver algum erro no cadastro, a mensagem será exibida na tela de cadastro

```jsp
<form action="cadastra">
	<label for="cpf">CPF:</label>
	<input id="cpf" type="text" name="usuario.cpf" />
	<input type="submit" value="Cadastrar" />
</form>
<c:forEach items="${errors}" var="error">
	${error.category} - ${error.message}<br />
</c:forEach>
```

Para saber mais sobre como utilizar validações no VRaptor, acesse a [[documentação do VRaptor|http://vraptor.caelum.com.br/documentacao/validacao/]]