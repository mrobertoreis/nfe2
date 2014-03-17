# Gerando Boletos

Veja um exemplo de geração de boleto para o Banco do Brasil
```java
public class Teste {  
    public static void main(String[] args) {  
        Datas datas = Datas.novasDatas()
                .comDocumento(1, 5, 2008)
                .comProcessamento(1, 5, 2008)
                .comVencimento(2, 5, 2008);  
        
        Emissor emissor = Emissor.novoEmissor()  
                .comCedente("Fulano de Tal")  
                .comAgencia("1824").comDigitoAgencia("4")  
                .comContaCorrente("76000")  
                .comNumeroConvenio("1207113")  
                .comDigitoContaCorrente("5")  
                .comCarteira("18")  
                .comNossoNumero("9000206");  
        
        Sacado sacado = Sacado.novoSacado()  
                .comNome("Fulano da Silva")  
                .comCpf("111.222.333-12")  
                .comEndereco("Av dos testes, 111 apto 333")  
                .comBairro("Bairro Teste")  
                .comCep("01234-111")  
                .comCidade("São Paulo")  
                .comUf("SP");  
        
        Banco banco = new BancoDoBrasil();  
        
        Boleto boleto = Boleto.novoBoleto()  
                .comBanco(banco)  
                .comDatas(datas)  
                .comEmissor(emissor)  
                .comSacado(sacado)  
                .comValorBoleto("200.00")  
                .comNumeroDoDocumento("1234")  
                .comInstrucoes("instrucao 1", "instrucao 2", "instrucao 3", "instrucao 4", "instrucao 5")  
                .comLocaisDePagamento("local 1", "local 2");  
        
        GeradorDeBoleto gerador = new GeradorDeBoleto(boleto);  
        
        // Para gerar um boleto em PDF  
        gerador.geraPDF("BancoDoBrasil.pdf");  
        
        // Para gerar um boleto em PNG  
        gerador.geraPNG("BancoDoBrasil.png");  
        
        // Para gerar um array de bytes a partir de um PDF  
        byte[] bPDF = gerador.geraPDF();  
        
        // Para gerar um array de bytes a partir de um PNG  
        byte[] bPNG = gerador.geraPNG();
    }  
}  
```

##Gerando um Documento PDF com varios Boletos

```java
Boleto[] boletos = {boletoDeJaneiro,boletoDeFevereiro,boletoDeMarco};  
GeradorDeBoleto gerador = new GeradorDeBoleto(boleto);
gerador.geraPDF("boletos.pdf"); 
```

De forma análoga, podemos também utilizar o recurso de varargs. 

```java
new GeradorDeBoleto(boletoDeJaneiro,boletoDeFevereiro,boletoDeMarco).geraPDF("boletos.pdf");
```

##Gerando Boleto em HTML

Em sistemas web você pode exibir o boleto diretamente em uma página, seguindo o seguinte passo a passo: 

* Caso esteja rodando seu sistema em Servlet 2.5 ou inferior, é necessário registrar a Servlet do Jasper Reports  no seu ```web.xml```.  Se estiver rodando em servlet 3.0 ou superior, esse passo é desnecessário, já que a servlet será registrada automaticamente:

```xml
<!-- configuração para servlet 2.5 -->
<servlet>
    <servlet-name>JasperReportsImageServlet</servlet-name>
    <servlet-class>net.sf.jasperreports.j2ee.servlets.ImageServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>JasperReportsImageServlet</servlet-name>
    <url-pattern>/stella-boleto</url-pattern>
</servlet-mapping>
```

* Para gerar o html dos boletos, use a classe ```GeradorDeBoletoHTML```:

```java
GeradorDeBoletoHTML gerador = new GeradorDeBoletoHTML(boleto);
```

* Agora é só escolher a forma de enviar o conteúdo do boleto para o usuário. Caso deseje enviar o pdf para exibição na página ou download, é só usar o método ```geraPDF```, que recebe como parâmetro um ```OutputStream```:

```java
GeradorDeBoletoHTML gerador = new GeradorDeBoletoHTML(boleto);
gerador.geraPDF(response.getOutputStream());
```

Já para exibir o conteúdo do boleto diretamente em html, use o método ```geraHtml```, que recebe como parametros um Writer, e o request:

```java
GeradorDeBoletoHTML gerador = new GeradorDeBoletoHTML(boleto);
gerador.geraHTML(response.getWriter(), request);
```

há outras variações desses métodos, para gravar os boletos gerados em arquivos ou arrays de bytes. Em caso de dúvidas de como usar qualquer um desses métodos consulte a documentação ou mande uma dúvida na nossa [[Lista de Discussão|listas-de-discussao]]

## Termos Relevantes

* **Aceite**: diz se o banco deve aceitar o boleto após a data de vencimento. Padrão: 'N'
* **Espécie de Documento**: identificador do tipo de boleto. Padrão: "DV"
* **Número do Documento**: código informado pelo banco para identificação do cliente
* **Carteira**: código informado pelo banco pra identificação do tipo do boleto
* **Número do Convênio**: código que identifica um emissor junto ao seu banco para associar seus boletos. Fornecido pelo banco
* **Nosso Número**: código que o cedente escolhe para manter controle sobre seus boletos. Esse valor serve para o cedente identificar quais boletos foram pagos ou não. Recomenda-se o uso de números sequênciais, na geração de diversos boletos, para facilitar a identificação dos boletos pagos
* **Cedente**: nome do emissor
* **Emissor**: pessoa/empresa que gera o boleto
* **Sacado**: pessoa/empresa que deve pagar o boleto