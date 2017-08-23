Importante o nome do arquivo é **Recibos.gs**!

Esté e o arquivo onde estará a função que trata os dados do formulário para gerar o recibo e enviar suas copias para o destinatário e para uma pasta do **Google Driver**. 

Nestá é a primeira vesão do script trata-se de uma unica função que faz todo procedimento. A única diferença dela para a [versão 2.0](recibos_m_planilha_rec2.md) é que na versão 2.0 os processos foram separados em funções especificas para facilitar a modificação e manutenção.



??? note "Abra para ver o código da função completo"
    ``` js
        function recibo() {
        var recibotemplateId = "14Zlj5zwYyWHhAUnBG9dMFYTzeClIa_xvayxJsB8k_Os"
        var past_recibos = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Recibos")
        var past_adm = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Pasta adm")

        var pasta_ramo = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_AESS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_CS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_CPMT = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_EMBS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_PES = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_RAS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_TEMS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");

        var dados_recibos = past_recibos.getDataRange().getValues();
        var ultimaLinha = past_recibos.getLastRow() - 1;

        var datarecibo = dados_recibos[ultimaLinha][2];
        var evento = dados_recibos[ultimaLinha][4];
        var unidade = dados_recibos[ultimaLinha][5];
        var nome_completo = dados_recibos[ultimaLinha][6];
        var CPF = dados_recibos[ultimaLinha][7];
        var valor = dados_recibos[ultimaLinha][10];
        var ano = dados_recibos[ultimaLinha][11];
        var mes = dados_recibos[ultimaLinha][12];
        var destinatariorecibo = dados_recibos[ultimaLinha][13];

        if (CPF.lenght != 11) {
            CPF = '0'.concat(CPF);
        }
        CPF = CPF.replace(/^(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");
        ano = ano.toString().right(2);
        var valorextenso = Extenso(valor);
        valor = valor.formatMoney(2, ',', '');
        destinatariorecibo = destinatariorecibo.toLowerCase();

        var idunidade, numrecibo, pasta_recibo;
        if (unidade == "Ramo") {
            idunidade = "00";
            numrecibo = pad(past_adm.getRange(2, 3).getValue(), 4);
            pasta_recibo = pasta_ramo;
        } else if (unidade == "AESS") {
            idunidade = "01";
            numrecibo = pad(past_adm.getRange(3, 3).getValue(), 4);
            pasta_recibo = pasta_AESS;
        } else if (unidade == "CS") {
            idunidade = "02";
            numrecibo = pad(past_adm.getRange(4, 3).getValue(), 4);
            pasta_recibo = pasta_CS;
        } else if (unidade == "CPMT") {
            idunidade = "03";
            numrecibo = pad(past_adm.getRange(5, 3).getValue(), 4);
            pasta_recibo = pasta_CPMT;
        } else if (unidade == "EMBS") {
            idunidade = "04";
            numrecibo = pad(past_adm.getRange(6, 3).getValue(), 4);
            pasta_recibo = pasta_EMBS;
        } else if (unidade == "PES") {
            idunidade = "05";
            numrecibo = pad(past_adm.getRange(7, 3).getValue(), 4);
            pasta_recibo = pasta_PES;
        } else if (unidade == "RAS") {
            idunidade = "06";
            numrecibo = pad(past_adm.getRange(8, 3).getValue(), 4);
            pasta_recibo = pasta_RAS;
        } else if (unidade == "TEMS") {
            idunidade = "07";
            numrecibo = pad(past_adm.getRange(9, 3).getValue(), 4);
            pasta_recibo = pasta_TEMS;
        }

        var idrecibo = idunidade + mes + numrecibo + ano + "ER";

        past_recibos.getRange(ultimaLinha + 1, 1).setValue(idrecibo);

        var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(idrecibo).getId();
        var docCopia = DocumentApp.openById(idCopia);
        var textoCopia = docCopia.getActiveSection();

        textoCopia.replaceText("NOME", nome_completo);
        textoCopia.replaceText("NUMEROCPF", CPF);
        textoCopia.replaceText("VALOR", valor);
        textoCopia.replaceText("VALEXTENSO", valorextenso);
        textoCopia.replaceText("CURSO", evento);
        textoCopia.replaceText("DATARECIBO", datarecibo);
        textoCopia.replaceText("RECIBO", idrecibo);
        docCopia.saveAndClose();

        var recibo_pdf = DriveApp.getFileById(idCopia).getAs("application/pdf");
        DriveApp.getFileById(idCopia).setTrashed(true);

        var pdf=  pasta_recibo.createFile(recibo_pdf);
        var urlarquivo = pdf.setSharing(DriveApp.Access.ANYONE, DriveApp.Permission.VIEW).getUrl()

        var html = HtmlService.createTemplateFromFile('rec_email_template');
        html.nome_completo = nome_completo;
        html.datarecibo = datarecibo;
        html.valor = valor;
        html.valorextenso = valorextenso
        html.evento = evento;
        html.idrecibo = idrecibo;
        html.urlpdf = urlpdf;
        var htmlBody = html.evaluate().getContent();
        var remetente = "IEEE UFABC<contato@ieeeufabc.org>";
        var assunto = "Recibo IEEE UFABC";
        MailApp.sendEmail(destinatariorecibo, assunto, html, {
            // bcc: 'outroemail@email.ext',
            name: remetente,
            htmlBody: htmlBody,
            attachments: recibo_pdf
        });

    }
    ```

### Acessando os documentos e locais necessários.

A primeira coisa que devemos fazer em nossa função é definir quais são os documentos que vamos utilizar durante o processo. 

``` js
    var recibotemplateId = "14Zlj5zwYyWHhAUnBG9dMFYTzeClIa_xvayxJsB8k_Os"
    var past_recibos = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Recibos")
    var past_adm = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Pasta adm")
```

Foram criadas 3 variáveis a primeira delas é `recibotemplateId` contém o Id único de um do meu arquivo de *template* criado anteriormente. 

As variáveis `plan_recibos` e `plan_adm` pegam a planilha ativa e selecionam respectivamente as pastas de trabalho "Recibos" e "Pasta adm". Ou seja quando eu chamar a variável `plan_recibos` ou `past_adm` eu estou chamando abrindo a pasta de trabalho correspondente.

!!! note ""
    No nosso caso pegar uma planilha ativa é um método funcional, pois o *Google Form* ativa uma planilha para escrever os dados quando recebe os dados e nos estaremos utilizando justamente essa planilha para trabalhar com os dados. Se desejar buscar a planilha pelo seu Id substitua `getActiveSpreadsheet()` por `openById(Id da planilha)`

    O comando `getSheetByName("Nome da pasta")` poderia ser substituído pelo comando `getSheets()["posição da pasta"]` sendo que a primeira pasta é tem a posição 0

A seguir adicionamos as variáveis que indicarão todas as possíveis pastas que para salvar meus recibos. O método utilizado para escolher qual destas pastas será utilizada para salvar o arquivo você pode ver na seção [Salvando os recibos no Driver](#salvando-recibo-no-driver).

```js
    var pasta_ramo = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_AESS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_CS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_CPMT = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_EMBS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_PES = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_RAS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
    var pasta_TEMS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
```

!!! note ""
    O comando `getFolderById("ID da pasta")` poderia ser substituído pelo comando `getFoldersByName("Nome da Pasta")`. O primeiro vai exatamente na pasta com aquele ID o segundo ficará varrendo o Google Driver atrás do de uma pasta com aquele nome.

### Extraindo dados da Pasta Recibos

Depois de indicar quais serão documentos vamos trabalhar devemos pegar os dados das planilhas para isso utilizamos aos seguintes comandos:

```js
    var dados_recibos = past_recibos.getDataRange().getValues();
    var ultimaLinha = past_recibos.getLastRow() - 1;
```

A variável `dados_recibos` pega toda os valores preenchidos na pasta de trabalho `plan_recibos` essas variáveis são guardadas em vetores numerados em que o vetor "0" armazena os dados da linha 1 da planilha. A variável ultima linha ela nos dá a ultima linha preenchida pasta de trabalho.

!!! important ""
    O comando `getLastRow()` me retorna um número, este número indica a ultima linha em branco da tabela de dados! Como este comando retorna um número eu posso fazer operações matemáticas com ele. Então para selecionar a ultima linha preenchida na variável ultima linha eu subtrai "um" deste número.

Ao seguirmos os passos acima transformamos nossa tabela de dados em uma Matriz de dados. Para extrair os dados da nossa matriz utilizamos o comando `matriz de dados[linha][coluna]`em que a célula A1 seria indicada pela linha 0 e coluna 0.

Os comandos abaixo se aplicados em uma planilha como a criada anteriormente guardam em suas variáveis os respectivos dados da ultima linha preenchida: Data do recibo, Evento, Unidade responsável, Nome completo, CPF, Valor pago, Ano da transação, Mês da transação e o e-mail registrado no formulário.

```js
// extrair dados
    var datarecibo = dados_recibos[ultimaLinha][2];
    var evento = dados_recibos[ultimaLinha][4];
    var unidade = dados_recibos[ultimaLinha][5];
    var nome_completo = dados_recibos[ultimaLinha][6];
    var CPF =dados_recibos[ultimaLinha][7];
    var valor = dados_recibos[ultimaLinha][10];
    var ano = dados_recibos[ultimaLinha][11];
    var mes = dados_recibos[ultimaLinha][12];
    var destinatariorecibo = dados_recibos[ultimaLinha][13];
```

### Tratando os dados extraídos

Para melhor efeito visual ou necessidade alguns dados devem ser tratados para isso utilizamos o seguinte algorítimo:

```js
    if (CPF.lenght != 11) {
        CPF = '0'.concat(CPF);
    }
    CPF = CPF.replace(/^(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");
    ano = ano.toString().right(2);
    var valorextenso = Extenso(valor);
    valor = valor.formatMoney(2, ',', '');
    destinatariorecibo = destinatariorecibo.toLowerCase();
```

O primeiro `if` é responsável por verificar se o CPF é uma *string* de 11 casas. caso não seja ele adiciona um 0 a esquerda. Posteriormente o CPF é formatado para o formato 123.456.789-11 por meio da expressão regular do algoritmo.

!!! note ""
    O CPF é armazenado como um número na planilha. Alguns CPF's iniciam com 0 então nestes casos o CPF ficaria com 10 algarismos. Para solucionar este problema o algoritmo transforma o CPF numa *string* verifica se ele tem 11 caracteres se não tiver adiciona um 0 a esquerda.

```js
    ano = ano.toString().right(2);
  
```

A variável `ano` é um número de 4 dígitos neste campo ela é transformada numa *string* e posteriormente dois caracteres da direta são armazenados.

```js  
  var valorextenso = Extenso(valor);
```

A variável `valorextenso` é a *string* que contém o número do campo "valor" escrito na sua forma extensa. Para fazermos isso é necessário utilizamos a função Extenso e para seu funcionamento correto é necessário que a variável `valor` seja definida como *Number*!

```js
    valor = valor.formatMoney(2, ',', '');
```

Reescrevo a variável valor no formato de *string* só que agora no formato com duas casas decimais utilizando o separador ",". É muito importante que essa linha venha depois da função Extenso.

```js
    destinatariorecibo = destinatariorecibo.toLowerCase();
```

Com a linha de código acima eu faço com que o e-mail seja escrito em caixa baixa.

A variável `idunidade` é escrito a partir do valor extraído e armazenado na variável `unidade`. Para determinar qual valor deve ser escrito é utilizada a seguinte função:

```js
    var idunidade;
    if (unidade == "Ramo") {
        idunidade = "00";
    } else if (unidade == "AESS") {
        idunidade = "01";
    } else if (unidade == "CS") {
        idunidade = "02";
    } else if (unidade == "CPMT") {
        idunidade = "03";
    } else if (unidade == "EMBS") {
        idunidade = "04";
    } else if (unidade == "PES") {
        idunidade = "05";
    } else if (unidade == "RAS") {
        idunidade = "06";
    } else if (unidade == "TEMS") {
        idunidade = "07";
    }
```

Veja que no inicio do algorítimo a variável `idunidade` é definida, mas só tem um valor atribuído quando ela respeita uma das condições de igualdade.

!!! attention ""
    Note que a estrutura utilizada neste laço `if` é semelhante nas seções [Tratando os dados extraídos](#tratando-os-dados-extraidos), [Extraindo dados da Pasta Adm](#extraindo-dados-da-pasta-adm) e [Salvando os recibos no Driver](#salvando-os-recibos-no-driver) então no algorítimo completo mostrado na seção [Função Recibo](#funcao-recibo) condensamos ele na forma mostrada na seção [If Elegante](#if-elegante).

### Extraindo dados da Pasta Adm

Ao criarmos a Pasta Adm na seção [Pasta Administrativa](#pasta-administrativa)indicamos qual é número que indica o próximo recibo emitido para pegarmos este número utilizamos o seguinte algorítimo:

```js
    var numrecibo;
    if (unidade == "Ramo") {
        numrecibo = pad(past_adm.getRange(2, 3).getValue(), 4);
    } else if (unidade == "AESS") {
        numrecibo = pad(past_adm.getRange(3, 3).getValue(), 4);
    } else if (unidade == "CS") {
        numrecibo = pad(past_adm.getRange(4, 3).getValue(), 4);
    } else if (unidade == "CPMT") {
        numrecibo = pad(past_adm.getRange(5, 3).getValue(), 4);
    } else if (unidade == "EMBS") {
        numrecibo = pad(past_adm.getRange(6, 3).getValue(), 4);
    } else if (unidade == "PES") {
        numrecibo = pad(past_adm.getRange(7, 3).getValue(), 4);
    } else if (unidade == "RAS") {
        numrecibo = pad(past_adm.getRange(8, 3).getValue(), 4);
    } else if (unidade == "TEMS") {
        numrecibo = pad(past_adm.getRange(9, 3).getValue(), 4);
    }
```

A variável `numrecibo`só tem um valor atribuído quando ela respeita uma das condições de igualdade. Esse valor será igual ao valor extraído pelo comando `getValue` da célula em indicada pelo comando `getRange(linha,coluna)` , em que o valor a célula A1 seria 1,1, formatado pela [Função pad(x,y)](#funcao-padxy) para ter sempre com 4 algorítimos.
Função pad(x,y)
!!! attention ""
    Note que a estrutura utilizada neste laço `if` é semelhante nas seções [Tratando os dados extraidos](#tratando-os-dados-extraidos), [Extraindo dados da Pasta Adm](#extraindo-dados-da-pasta-adm) e [Salvando os recibos no Driver](#salvando-os-recibos-no-driver) então no algorítimo completo mostrado na seção [Função Recibo](#funcao-recibo) condensamos ele na forma mostRada na seção [If Elegante](#if-elegante).

### Definindo e registrando o ID do Recibo

Quando criamos nossa planilha deixamos a coluna "A" definida com . Este será preenchido por um conjunto de caracteres respeitando o seguinte código:

``` markdown  
O  do arquivo é CPmm0000yyTK em que:
CP - Unidade em código numérico
mm - Mês da movimentação
0000 - Numero da movimentação
yy - Ano da movimentação
t - **E**ntrada ou **S**aída
K - **M**ovimentação, **R**ecibo, **T**ransferência
```

A variável `idrecibo` para este algorítimo é definida definida pela concatenação de variáveis como é mostrado no código abaixo.

```js
    var idrecibo = idunidade + mes + numrecibo + ano + "ER";
```

O comando `setValue("valor")` mostrado na linha de código abaixo é responsável por escrever o `idrecibo` na primeira coluna da linha que estamos trabalhando.

```js
   past_recibos.getRange(ultimaLinha+1, 1).setValue(idrecibo);
```

!!! attention ""
    Comando getRange(linha, coluna) tem a célula "A1" como linha 1 e coluna 1 diferentemente do que foi considerado na seção **Extraindo dados da Pasta Recibos**. A soma de uma unidade (`+1`)   no campo referente a linha é para "corrigir" está característica.

### Construindo o Recibo

Para construir o Recibo iremos utilizar o *template* criado na seção [Doc](#doc) juntamente com os dados extraídos e tratados nos passo anteriores.

Para isso devemos criar uma copia temporária do recibo e pegar o numero de ID dela  `idcopia`, abrir essa e armazenar essa cópia na variável ``docCopia`` para então copiar o texto deste documento na variável `textoCopia`. Para fazer isso utilizamos as seguintes funções:

```js
    var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(idrecibo).getId();
    var docCopia = DocumentApp.openById(idCopia);
    var textoCopia = docCopia.getActiveSection();
```

Agora que temos o texto armazenado na variável `textoCopia` iremos substituir partes deste texto pelo valor das variáveis correspondentes feito isso iremos salvar e fechar o documento `docCopia`. Para isso utilizamos os seguintes comandos:

```js
    textoCopia.replaceText("NOME", nome_completo);
    textoCopia.replaceText("NUMEROCPF", CPF);
    textoCopia.replaceText("VALOR", valor);
    textoCopia.replaceText("VALEXTENSO", valorextenso);
    textoCopia.replaceText("CURSO", evento);
    textoCopia.replaceText("DATARECIBO", datarecibo);
    textoCopia.replaceText("RECIBO", idrecibo);
    docCopia.saveAndClose();
```

Agora utilizamos comando abaixo para pegar o arquivo por meio de seu ID armazenado na variável `idCopia` para converter para PDF e armazenar o PDF na variável `recibo_pdf`. Com está ultima variável podemos fazer operações como salva-lo no drive e/ou envia-lo por e-mail.

```js
    var recibo_pdf = DriveApp.getFileById(idCopia).getAs("application/pdf");
```

Depois de converter para o formato ".pdf" e o armazenar em uma variável eu posso deletar o arquivo temporário criado para isso utilizo o comando:

```js
  DriveApp.getFileById(idCopia).setTrashed(true);
```
### Salvando os Recibos no Driver

Na seção [Acessando os documentos e locais necessários](#acessando_os documentos_e_locais_ _necessarios) definimos as pastas que poderiam ser utilizadas para salvar nosso recibo agora iremos escolher qual pasta iremos salvar. Para isso utilizo o seguinte sequencia de comandos:

```js
    var pasta_recibo;
    if (unidade == "Ramo") {
        pasta_recibo = pasta_ramo;
    } else if (unidade == "AESS") {
        pasta_recibo = pasta_AESS;
    } else if (unidade == "CS") {
        pasta_recibo = pasta_CS;
    } else if (unidade == "CPMT") {
        pasta_recibo = pasta_CPMT;
    } else if (unidade == "EMBS") {
        pasta_recibo = pasta_EMBS;
    } else if (unidade == "PES") {
        pasta_recibo = pasta_PES;
    } else if (unidade == "RAS") {
        pasta_recibo = pasta_RAS;
    } else if (unidade == "TEMS") {
        pasta_recibo = pasta_TEMS;
    }
```

!!! attention ""
    Note que a estrutura utilizada neste laço `if` é semelhante nas seções [Tratando os dados extraidos](#tratando-os-dados-extraidos), [Extraindo dados da Pasta Adm](#extraindo-dados-da-pasta-adm) e [Salvando os recibos no Driver](#salvando-os-recibos-no-driver) então no algorítimo completo mostrado na seção [Função Recibo](#funcao-recibo) condensamos ele na forma mostrada na seção [If Elegante](#if-elegante).

Para salvar o recibo na pasta correta e pegar o URL deste arquivo para compartilhamento utilizamos as funções abaixo:

``` js
        var pdf = pasta_recibo.createFile(recibo_pdf)
        var urlpdf = pdf.setSharing(DriveApp.Access.ANYONE, DriveApp.Permission.VIEW).getUrl()
```

??? note "Se voce tiver uma unica pasta!"
    Caso todos os recibos sejam salvos apenas em uma pasta substituimos todas as pastas definidas na seçao [Acessando os documentos e locais necessários](#acessando-os-documentos-e-locais-necessarios) pela função: ` var pasta_recibo = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");`

    O laço `if` responsável pela escolha das pastas na seção [Salvando os recibos no Driver](#salvando-os-recibos-no-driver) ou os campos que fazem essa função no algorítimo da seção [If Elegante](#if-elegante) devem ser removidos.

### Construindo e enviando o E-mail

O corpo do e-mail é feito com o *template* Html feito na seção [Modelo HTML](#modelo-html) para escreve-lo utilizamos o seguinte algorítimo:

```js
    var html = HtmlService.createTemplateFromFile('rec_email_template');
    html.nome_completo = nome_completo;
    html.datarecibo = datarecibo;
    html.valor = valor;
    html.valorextenso = valorextenso
    html.evento = evento;
    html.idrecibo = idrecibo;
    html.urlpdf = urlpdf;
    var htmlBody = html.evaluate().getContent();    
```

Na primeira linha do algorítimo eu crio a variável `html` e nela armazeno um *template* que utiliza como modelo o arquivo criado anteriormente.
Nas linhas seguintes eu falo que as variáveis desse modelo são iguais as armazenadas pelo *script*.

```
htlm.variavel_template = variavel_script
```

Na ultima linha eu crio a variável `htmlBody` nela armazeno o conteúdo que da variável `html`.

Posteriormente defino as variáveis `remetente` e `assunto` que armazenam respectivamente o endereço de e-mail que irá aparecer como remetente do e-mail e o assunto do e-mail.
```js
    var remetente = "IEEE UFABC<contato@ieeeufabc.org>";
    var assunto = "Recibo IEEE UFABC";
```



Por fim utilizo a função abaixo para enviar o e-mail para o destinatário do recibo. O destinatário do recibo já foi definido na seção [Extraindo dados da Pasta Recibos](#extraindo-dados-da-pasta-recibos), caso você deseje enviar por padrão uma cópia deste e-mail para outro destinatário basta modificar o campo `bcc` comentado no algorítimo abaixo:

```js   
    MailApp.sendEmail(destinatariorecibo, assunto, html, {
        // bcc: 'outroemail@email.ext',
        name: remetente,
        htmlBody: htmlBody,
        attachments: recibo_pdf
    });
```

## If Elegante

Nas seções [Tratando os dados extraídos](#tratando-os-dados-extraidos), [Extraindo dados da Pasta Adm](#extraindo-dados-da-pasta-adm) e [Salvando os recibos no Driver](#salvando-os-recibos-no-driver) definimos as variáveis `idunidade`, `numrecibo` e `pasta_recibo` por meio de um laço `if` a estrutura utilizada nestes 3 laços foi a mesma então para maior elegância do código no algorítimo completo que mostrado na seção [Função Recibo](#funcao-recibo) utilizamos a versão mostrada abaixo;

``` js
    var idunidade;
    var numrecibo;
    var pasta_recibo;
    if (unidade == "Ramo") {
        idunidade = "00";
        numrecibo = pad(past_adm.getRange(2, 3).getValue(), 4);
        pasta_recibo = pasta_ramo;
    } else if (unidade == "AESS") {
        idunidade = "01";
        numrecibo = pad(past_adm.getRange(3, 3).getValue(), 4);
        pasta_recibo = pasta_AESS;
    } else if (unidade == "CS") {
        idunidade = "02";
        numrecibo = pad(past_adm.getRange(4, 3).getValue(), 4);
        pasta_recibo = pasta_CS;
    } else if (unidade == "CPMT") {
        idunidade = "03";
        numrecibo = pad(past_adm.getRange(5, 3).getValue(), 4);
        pasta_recibo = pasta_CPMT;
    } else if (unidade == "EMBS") {
        idunidade = "04";
        numrecibo = pad(past_adm.getRange(6, 3).getValue(), 4);
        pasta_recibo = pasta_EMBS;
    } else if (unidade == "PES") {
        idunidade = "05";
        numrecibo = pad(past_adm.getRange(7, 3).getValue(), 4);
        pasta_recibo = pasta_PES;
    } else if (unidade == "RAS") {
        idunidade = "06";
        numrecibo = pad(past_adm.getRange(8, 3).getValue(), 4);
        pasta_recibo = pasta_RAS;
    } else if (unidade == "TEMS") {
        idunidade = "07";
        numrecibo = pad(past_adm.getRange(9, 3).getValue(), 4);
        pasta_recibo = pasta_TEMS;          
    }
```