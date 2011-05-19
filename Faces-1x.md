# Validando Documentos com Stella-Faces

O Stella Faces fornece componentes formatadores e validadores JSF para a sua aplicação.

Veja os exemplos de validação e note a simplicidade dos códigos implementados.

Partindo de uma aplicação configurada para JSF podemos começar a utilizar nosso validador na página. O Stella Faces fornece uma biblioteca de tags para facilitar o trabalho do desenvolvedor, sendo necessário importar as taglibs para a versão do JSF que estiver utilizando


### Taglibs para JSF 1x

```jsp
<!-- taglib para Jsf 1x, com JSP -->
<%@ taglib uri="http://stella.caelum.com.br/faces" prefix="stella"%>  
```

```jsp
<!-- taglib para Jsf 1x, utilizando facelets -->
<html xmlns="http://www.w3.org/1999/xhtml"  
      xmlns:ui="http://java.sun.com/jsf/facelets"  
      xmlns:h="http://java.sun.com/jsf/html"  
      xmlns:f="http://java.sun.com/jsf/core"  
      xmlns:stella="http://stella.caelum.com.br/faces">  
```

### Taglibs para JSF 2x

```jsp
<!-- taglib para Jsf 2x, utilizando facelets -->
<html xmlns="http://www.w3.org/1999/xhtml"  
      xmlns:ui="http://java.sun.com/jsf/facelets"  
      xmlns:h="http://java.sun.com/jsf/html"  
      xmlns:f="http://java.sun.com/jsf/core"  
      xmlns:stella="http://stella.caelum.com.br/faces2">  
```
A unica mudança é o namespace que muda de http://stella.caelum.com.br/faces para http://stella.caelum.com.br/faces2

## Utilizando as taglibs do stella
```jsp
<f:view>  
    <h:form id="formulario">  
        <h:panelGrid>  
            <h:outputLabel value="cpf" for="cpf" />  
            <h:inputText id="cpf" value="cpf">  
                <stella:validateCPF />  <!-- Com esta linha realizamos a validação -->
            </h:inputText>  
            <h:message for="cpf" />  
        </h:panelGrid>  
        <h:commandButton value="Enviar" />  
    </h:form>  
</f:view>  
```

## Validação com facelets

Crie uma página com extensão .xhtml como o exemplo abaixo. Lembre-se de configurar os xmls apropriadamente.
```jsp
<f:view>  
    <h:form>  
        <h:panelGrid>  
            <h:outputLabel for="cpf" value="CPF:" />  
            <h:inputText id="cpf" value="cpf">  
                <stella:validateCPF formatted="true"/>  
            </h:inputText>  
            <h:message for="cpf" />  
        </h:panelGrid>  
        <h:commandButton value="Enviar" />  
    </h:form>
</f:view>  

```

## Utilizando o validatorId

Outra maneira de realizar a validação é inserindo o validador através da tag `validator` e seu **validatorId**. Por sua complexidade desnecessária, essa abordagem é menos recomendada.
```jsp
<h:inputText id="cpf">  
    <f:validator validatorId="StellaCPFValidator" />  
</h:inputText>  
```

## Exemplo de uso de validação de Inscrição Estadual

```jsp
<h:panelGrid>  
    <h:messages />  
    <h:outputText value="Selecione um estado:" />  

    <h:selectOneMenu id="estado" 
        valueChangeListener="{EmpresaBean.atualizaEstadoNoValidador}" immediate="true">  
        <f:selectItem itemValue="" itemLabel="" />  
        <f:selectItem itemValue="SP" itemLabel="SP" />  
        <f:selectItem itemValue="RJ" itemLabel="RJ" />  
    </h:selectOneMenu>  

    <h:outputLabel value="IE sem formatacao:" for="ie" />  
    <h:inputText id="ie" value="{EmpresaBean.ie}">  
        <stella:validateIE formatted="false" binding="{EmpresaBean.ieValidator}" />  
    </h:inputText>  
</h:panelGrid>
```

No componente "estado", o `valueChangeListener` precisa ser `immediate="true"`, para que o valor do estado seja preenchido no componente validador, antes da validação ocorrer.

Caso o componente não tenha `immediate="true"`, o valor do estado será preenchido no componente validador apenas depois da validação ter ocorrido.

Repare no método `atualizaEstadoNoValidador` do bean abaixo.

```java
public class EmpresaBean {  

    private StellaIEValidator ieValidator;  

    private String ie;  

    private String ieFormatado;  

    public void atualizaEstadoNoValidador(ValueChangeEvent event) {  
        this.ieValidator.setEstado(event.getNewValue().toString());  
    }  

    public StellaIEValidator getIeValidator() {  
        return ieValidator;  
    }  

    public void setIeValidator(StellaIEValidator ieValidator) {  
        this.ieValidator = ieValidator;  
    }  

    //getters & setters  
}
```