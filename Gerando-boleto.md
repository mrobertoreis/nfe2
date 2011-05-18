# Gerando Boletos

Veja um exemplo de geração de boleto para o Banco do Brasil
```java
public class Teste {  
    public static void main(String[] args) {  
    Datas datas = Datas.newDatas()
            .withDocumento(1, 5, 2008)
            .withProcessamento(1, 5, 2008)
            .withVencimento(2, 5, 2008);  

    Emissor emissor = Emissor.newEmissor()  
            .withCedente("Fulano de Tal")  
            .withAgencia(1824).withDvAgencia('4')  
            .withContaCorrente(76000)  
            .withNumConvenio(1207113)  
            .withDvContaCorrente('5')  
            .withCarteira(18)  
            .withNossoNumero(9000206);  

    Sacado sacado = Sacado.newSacado()  
            .withNome("Fulano da Silva")  
            .withCpf("111.222.333-12")  
            .withEndereco("Av dos testes, 111 apto 333")  
            .withBairro("Bairro Teste")  
            .withCep("01234-111")  
            .withCidade("São Paulo")  
            .withUf("SP");  

    Banco banco = new BancoDoBrasil();  

    Boleto boleto = Boleto.newBoleto()  
            .withBanco(banco)  
            .withDatas(datas)  
            .withDescricoes("descricao 1", "descricao 2", "descricao 3", "descricao 4", "descricao 5")  
            .withEmissor(emissor)  
            .withSacado(sacado)  
            .withValorBoleto("200.00")  
            .withNoDocumento("1234")  
            .withInstrucoes("instrucao 1", "instrucao 2", "instrucao 3", "instrucao 4", "instrucao 5")  
            .withLocaisDePagamento("local 1", "local 2")  
            .withNoDocumento("4343");

    BoletoGenerator gerador = new BoletoGenerator(boleto);  

    // Para gerar um boleto em PDF  
    gerador.toPDF("BancoDoBrasil.pdf");  

    // Para gerar um boleto em PNG  
    gerador.toPNG("BancoDoBrasil.png");  

    // Para gerar um array de bytes a partir de um PDF  
    byte[] bPDF = gerador.toPDF();  

    // Para gerar um array de bytes a partir de um PNG  
    byte[] bPNG = gerador.toPNG();  

    }  
}  
```

##Gerando um Documento PDF com varios Boletos

```java
Boleto[] boletos = {boletoDeJaneiro,boletoDeFevereiro,boletoDeMarco};  
BoletoGenerator gerador = new BoletoGenerator(boletos);  
gerador.toPDF("boletos.pdf");  
```

De forma análoga, podemos também utilizar o recurso de var args. 

```java
new BoletoGenerator(boletoDeJaneiro,boletoDeFevereiro,boletoDeMarco).toPDF("boletos.pdf");
```

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