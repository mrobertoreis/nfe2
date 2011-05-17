# Anotando o modelo

Se você já está familiarizado com as anotações, esta sintaxe será familiar.

```java
public class Empresa {  

    // não nulo com 20 caracterers no máximo  
    @Length(max=20)  
    @NotNull
    private String nome;

    private String cnpj;

    public String getNome() {  
        return nome;  
    }  

    // Uma representação válida par CNPJ, não nula.
    // Podemos anotar tanto os atributos quanto seus métodos getters
    @CNPJ  
    @NotNull  
    public String getCnpj() {  
        return cnpj;  
    }  
}
```

O exemplo acima mostra apenas validação em propriedades públicas, mas você também pode anotar campos com qualquer visibilidade.
```java
public class Pessoa {  

    private String nome;  
    
    @NotNull   
    @CPF
    private String cpf;  

    ... 
} 
```

Você pode também inserir anotações em interfaces. O Bean Validation irá verificar todas as superclasses e interfaces extendidas ou implementadas por qualquer bean fornecido para carregar as anotações apropriadas. 

```java
public interface EmpresaPrivada {  

    @NIT  
    String getPis();  
    ...  
}  

public class Acme implements EmpresaPrivada {  

    public String getPis(){...}  
    ...  
}  
```
A propriedade PIS será verificada quando a classe *Acme* for validada.

## Exemplo de validação de IE

A validação de Inscrição Estadual é um pouco diferente, já que além do número da IE também é necessário informar qual o estado, já que as regras mudam de estado para estado.
```java
@IE  
public class PrestadoraDeServicos {  

    private String ie;  

    private String estado;  

    public PrestadoraDeServicos(String ie, String estado) {  
        this.ie = ie;  
        this.estado = estado;  
    }  

    public String getIe() {  
        return ie;  
    }  

    public String getEstado() {  
        return estado;  
    }  
} 
```

Por padrão o validador busca os dados através dos métodos getIe() e getEstado(). Opcionalmente, podemos redefinir as propriedades através da anotação. 

```java
@IE(ieField="inscricaoEstadual" , estadoField="estadoDaInscricaoEstadual")  
public class PrestadoraDeServicos {  

    private String ie;  
    private String estado;  

    public PrestadoraDeServicos(String ie, String estado) {  
        this.ie = ie;  
        this.estado = estado;  
    }  

    public String getInscricaoEstadual() {  
        return ie;  
    }  

    public String getEstadoDaInscricaoEstadual() {  
        return estado;  
    }  
}  
```

