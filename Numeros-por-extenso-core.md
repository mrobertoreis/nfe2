# Números por extenso
## Transformando Algarismos em Palavras

Veja abaixo como utilizar a rotina de extenso do Caelum Stella.
```java
NumericToWordsConverter converter;  
converter = new NumericToWordsConverter(new FormatoDeInteiro());  
double numero = 1000150.99;  
String extenso = converter.toWords(numero);  
System.out.println(extenso);
// um milhão e cento e cinquenta inteiros e novecentos e noventa milésimos 
```

## Utilizando o Formato De Real

Utilizando o stella apra imprimir valores monetários
```java
NumericToWordsConverter converter;  
converter = new NumericToWordsConverter(new FormatoDeReal());  
double numero = 1000150.99;  
String extenso = converter.toWords(numero);  
System.out.println(extenso);
// um milhão e cento e cinquenta reais e noventa e nove centavos
```

## Utilizando um Formato Próprio

Primeiramente criamos um formato próprio implementando a interface `FormatoDeExtenso`. Vamos criar um formato para imprimir tempo em segundos: 
```java
public class FormatoDeSegundosComMilesimos implements FormatoDeExtenso {  

    public int getCasasDecimais() {  
        return 3;  
    }  

    public String getUnidadeDecimalNoPlural() {  
        return "milésimos de segundo";  
    }  

    public String getUnidadeDecimalNoSingular() {  
        return "milésimo de segundo";  
    }  

    public String getUnidadeInteiraNoPlural() {  
        return "segundos";  
    }  

    public String getUnidadeInteiraNoSingular() {  
        return "segundo";  
    }  

}  
```

Agora vamos utilizar este formato para extrair números por extenso. 
```java
public class ExemploDeExtenso {  

    public static void main(String[] args) {  
        FormatoDeExtenso formato = new FormatoDeSegundosComMilesimos();  
        NumericToWordsConverter converter = new NumericToWordsConverter(formato);  
        String tempo = converter.toWords(0.05);  
        String message = "Cibernautas demoram " + tempo + " a avaliar uma página.";  
        System.out.println(message);  

        tempo = converter.toWords(9.52);  
        message =   "Técnico diz que Bolt poderia ter feito 100m em " + tempo + ".";  
        System.out.println(message);  
    }  
}  
```
A saída no console será a seguinte:

*Cibernautas demoram cinquenta milésimos de segundo a avaliar uma página.*

*Técnico diz que Bolt poderia ter feito 100m em nove segundos e quinhentos e vinte milésimos de segundo.*



