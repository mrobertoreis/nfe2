# Validação na camada de persistência

Ao utilizarmos as anotações do Stella, o Bean Validation se encarrega de aplicar as restrições no schema do banco de dados e de verificar a validade antes de inserir ou atualizar instâncias.

Além de inserir as anotações, o que é preciso fazer para manter válida a inserção e atualização no banco de dados?

Nada. Lembre-se do princípio **DRY**. As restrições de validação estão, de alguma forma, descritas pela anotação fornecida. **Qualquer implementação da JPA** é capaz de realizar a validação através dessa descrição.

## Como habilitar a validação com Hibernate

Por padrão o Hibernate está habilitado a disparar as validações quando tenta persistir uma instância.

O Hibernate Validator trás dois listeners de eventos do Hibernate, embutidos. Sempre que um `PreInsertEvent` ou `PreUpdateEvent` ocorre, os listeners irão verificar todas as restrições da instância da entidade e lançar uma exceção se qualquer uma delas for violada.

Basicamente, objetos serão verificados antes de qualquer inserção ou atualização feitas pelo provedor Hibernate. Isto inclui as modificações aplicadas em cascata! **Este é o meio mais conveniente e fácil para ativar o processo de validação**. Ao encontrar uma violação nas restrições, o evento irá lançar uma `ConstraintViolationException` que contém um set de valores inválidos (`ConstraintViolation`) descrevendo cada falha. Esse set pode ser obtido através do método `getConstraintViolations()`.

Se o Hibernate Validator estiver presente no **classpath**, o Hibernate Annotations (ou Hibernate EntityManager) o utilizará de forma transparente. Se, por algum motivo, você quiser desabilitar esta integração, configure *hibernate.validator.autoregister_listeners* como *false*.

## Validação com outro provedor de JPA

O Bean Validation não está preso ao Hibernate para validação baseada em eventos: está disponível um Entity Listener da camada de persistência do Java. Sempre que uma entidade escutada é persistida ou atualizada, o Bean Validation irá verificar todas as restrições dessa instância e lançar uma exceção se qualquer restrição for violada. Basicamente, objetos serão verificados antes de qualquer inserção/atualização feitas pelo provedor JPA. Isto inclui as modificações aplicadas em cascata! Ao encontrar uma violação nas restrições, o evento sempre irá lançar uma `ConstraintViolationException`, e as mensagens específicas de cada validação podem ser obitidas da mesma maneira descrita anteriormente.

Veja como tornar uma classe passível de validação:
```java
@Entity  
@EntityListeners( JPAValidateListener.class )  
public class Submarino {  
    ...  
}  
```

Note que se optar por não usar o Hibernate, precisará definir o `EntityListener` em toda entidade a ser validada.