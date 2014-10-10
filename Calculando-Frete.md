# Calculando Frete

Veja o exemplo de como calcular o frete de uma encomenda:

```java
public class Teste {
	public static void main(String[] args) {
		try{
			Encomenda encomenda = new Encomenda()
			                       .doCep("20021180")
			                       .paraOCep("01205-000")
			                       .comAltura("10")
			                       .comComprimento("20")
			                       .comLargura("20")
			                       .comPeso(0.3)
			                       .comAvisoDeRecebimento()
			                       .semMaoPropria();

			Frete  frete = CalculoFreteCorreio.calcularFrete(encomenda, Servico.SEDEX);
			System.out.println(String.format("Entregará por %s reais em até %s %s .", 
					frete.getValor(),frete.getPrazoEntrega(),frete.getPrazoEntrega() <=1 ? "dia útil":"dias úteis"));
		}catch(CorreiosException e){
			System.out.println("Código do erro: " + e.getCodigo());
			System.out.println("Motivo        : " + e.getMensagem());
		}
	}
}
```



Caso tenha um padrão para a encomenda (sempre o mesmo tamanho, do mesmo CEP,mesmo peso etc), basta configura-lo no arquivo encomenda.propeties e informar apenas o CEP de destino:

```properties
#encomenda.propeties
br.com.caelum.stella.frete.encomenda.ceporigem=7002900
br.com.caelum.stella.frete.encomenda.peso=0.250
br.com.caelum.stella.frete.encomenda.formato=1
br.com.caelum.stella.frete.encomenda.comprimento=20
br.com.caelum.stella.frete.encomenda.altura=2
br.com.caelum.stella.frete.encomenda.largura=11
br.com.caelum.stella.frete.encomenda.diametro=0
br.com.caelum.stella.frete.encomenda.maopropria=n
br.com.caelum.stella.frete.encomenda.valordeclarado=0
br.com.caelum.stella.frete.encomenda.avisorecebimento=n
```

E para utilizar
```java
CalculoFreteCorreio.calcularFrete(new Encomenda().paraOCep("04101-300"),  Servico.PAC);
```