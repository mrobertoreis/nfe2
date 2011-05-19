# Validando Documentos com Stella-Faces

O Stella Faces fornece componentes formatadores e validadores JSF para a sua aplicação.

Veja os exemplos de validação e note a simplicidade dos códigos implementados.

Caelum Stella Faces fornece componentes validadores para documentos brasileiros. Dessa forma, você não precisa mais se preocupar com este código a cada novo projeto que começar!

Partindo de uma aplicação configurada para JSF podemos começar a utilizar nosso validador na página.

## Utilizando as taglibs do stella

O Stella Faces fornece uma biblioteca de tags para facilitar o trabalho do desenvolvedor.
```jsp
<!-- Aqui declaramos a taglib do stella -->
<%@ taglib uri="http://stella.caelum.com.br/faces" prefix="stella"%>  
<%@ taglib uri="http://java.sun.com/jsf/core" prefix="f"%>  
<%@ taglib uri="http://java.sun.com/jsf/html" prefix="h"%>  
<html>  
    <head>  
        <title>Validando com Custom Tag do Caelum Stella-Faces</title>  
    </head>  
    <body>  
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
    </body>  
</html>
```

## Validação com facelets

Crie uma página com extensão .xhtml como o exemplo abaixo. Lembre-se de configurar os xmls apropriadamente.
```jsp
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">  
<html xmlns="http://www.w3.org/1999/xhtml"  
      xmlns:ui="http://java.sun.com/jsf/facelets"  
      xmlns:h="http://java.sun.com/jsf/html"  
      xmlns:f="http://java.sun.com/jsf/core"  
      xmlns:stella="http://stella.caelum.com.br/faces">  
    <head>  
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />  
        <title>Validando com Facelets</title>  
    </head>  
    <body>  
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
    </body>  
</html>
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