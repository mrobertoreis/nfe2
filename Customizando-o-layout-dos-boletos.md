A forma mais simples para customizar os templates utilizados para gerar os boletos é realizar o download do **Ireport** ou do **JasperStudio**, ambos disponíveis para download na [página de downloads da JasperSoft](http://community.jaspersoft.com/download). São ferramentas que permitirão a você editar o layout dos boletos de maneira gráfica, sendo mais simples de utilizar do que editar manualmente o xml com o template.

Após o download e instalação da ferramenta escolhida, basta abrir os arquivos **jrxml** que contém os templates usados por padrão no Stella-Boleto  e edita-los para atender suas necessidades. Você pode encontrar esses arquivo dentro da pasta [[templates|https://github.com/caelum/caelum-stella/tree/master/stella-boleto/src/main/resources/br/com/caelum/stella/boleto/templates]] do código fonte. São dois arquivos, um com o layout principal do boleto (_boleto-default.jrxml_) e outro com um subrelatório para exibição das instruções do boleto (_boleto-default_instrucoes.jrxml_).

Após a edição desses arquivos, precisamos agora instruir o Stella-Boleto a utilizar o template customizado no lugar do template default. A classe ```GeradorDeBoleto``` e seus derivados possui um construtor que recebe como parâmetro qual o template a ser utlizado, além de um ```Map``` para passagem de parâmetros opcionais para o template. 

Por exemplo, imagine que seus layouts customizados estão disponíveis na pasta ```\WEB-INF\jasper```. O seguinte codigo pode ser usado para carregar esses layouts:

```java
//Mapa para parâmetros
Map<String, Object> parametros = new HashMap<String, Object>();

//carrega o caminho físico do arquivo
String reportPath = request.getServletContext().getRealPath("/WEB-INF/jasper/boleto-custom.jasper");

//carrega o conteúdo do arquivo em um InputStream
InputStream templateBoleto = new FileInputStream(reportPath);

// passa para o gerador de boleto os dados do template no construtor, 
// junto com o mapa com os parâmetros, além dos dados dos boletos
GeradorDeBoletoHtml new GeradorDeBoletoHTML(templateBoleto,parametros,boleto);
```

Todo parâmetro passado nesse mapa ficará disponível para ser utilizado dentro do template. Por exemplo, para incluir um logotipo, bastaria criar um parâmetro chamado **LOGO** no template, que poderia ser passado da seguinte maneira:

```java
parametros.put("LOGO", new FileInputStream("logo.png"));
```
