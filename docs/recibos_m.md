# Estrutura do Documento
Para o devido funcionamento deste script é necessario de 4 arquivos:

 * Uma "Formulário Google". - Ele servirá de entrada de dados do sistema é o unico "arquivo" que o usuário final terá acesso.
 * Uma "Planilha Google" - Ela é seu banco de dados, aqui serão depositadas todas as respostas do seu formulário, é onde serão feitas algumas operações necessarias e o script que fará as funções mais avançadas é criado nesta planilha.
 * Um Script. - Ele é criado diretamente na planilha, ele é responsável pelas interações entre APIs do Google.
 * Um "Documentos Google". - Ele é o modelo utilizado para gerar o recibo propriamente dito.

## O formuário

Devido a necessidade de modificações constantes (adicionar ou retirar itens) deste formulário optou-se pela utilização de um "Formumário Google", pois com ele é facil modificar o conteúdo dos seus campos.
Todos os campos do formulário são de preenchimento obrigatório.

### Senha de acesso:
Este campo é um campo de validação necessário para garantir que apenas membros do IEEE possam utilizar o formulário:

!!! note  ""
	Titulo do campo: Senha de acesso.

	Tipo de campo: Resposta curta

	Validada por uma expressão regular* do tipo: 

	​``` 
	^suasenha$
	​```
    
	Mensagem de aviso em caso de erro: Senha inválida!

Para modificar a senha você deve moficar a parte textual do validador. Os simbolos `^` e `$` são comandos da expressão regular. No exemplo da figura a o usuário deve escrever "suasenha" para conseguir validar a resposta.

[![rescibossenha](imagens/Recibos/cr_senha.png)](imagens/Recibos/cr_senha.png)


### Referente à:
Neste Campo selecionarei o motivo gerador do recibo, ou seja o que o destinatário do recibo está adquirindo. 

!!! note  ""
	Titulo do campo: Referente à (ao):

	Tipo de campo: Lista suspensa


Para adicionar um campo basta digitar o produto na linha "Adicionar opção". É importante que o texto escrito neste campo faça sentido quando se ler a segunte frase ".... referente a(o) **OPÇÃO**". Para remover um campo basta clicar no "X" que está à direita. Na figura abaixo é possivel visualizar estas ações.

[![refetentea](imagens/Recibos/cr_refetentea.png)](imagens/Recibos/cr_refetentea.png)

### Unidade Responsável:
Campo em que é será selecionada a unidade que está recebendo do dinheiro referente ao recibo.

!!! note  ""
	Titulo do campo: Unidade Responsável:

	Tipo de campo: Lista suspensa


Para adicionar um campo basta digitar o produto na linha "Adicionar opção". É de extrema importência que utilizem apenas as siglas da unidade para referenciar ela. Para remover um campo basta clicar no "X" que está à direita. 

### Nome Completo

Local onde deverá ser preenchido o nome completo da pessoa que receberá o recibo.

!!! note  ""
	Titulo do campo: Come completo.

	Tipo de campo: Resposta curta

[![nome](imagens/Recibos/cr_nome.png)](imagens/Recibos/cr_nome.png)

### CPF

Campo para registro dos numéros do CPF da pessoa que adquiriu que receberá o recibo.

!!! note  ""
	Titulo do campo: CPF.

	Tipo de campo: Resposta curta

	Descrição: Insira apenas os números CPF.

	Validada por uma expressão regular do tipo: correspondente à:

	​``` 
	([0-9]{11})
	​``` 
	
	Mensagem de aviso em caso de erro: Formato de CPF inválido!

[![CPF](imagens/Recibos/cr_CPF.png)](imagens/Recibos/cr_CPF.png)

A expressão regular me diz para escrever 11 digitos numéricos qualquer coisa diferente disso ela me retornará a mensagem de erro.

### Valor

Este campo é onde devemos digitar o valor do recibo.

!!! note  ""
	Titulo do campo: Valor pago em Reais.

	Tipo de campo: Resposta curta

	Descrição: Valor deve ser dado em Reais e sempre positivo. Caso necessário, utilize o separador ponto. Se o evento for gratuito, digite 0.

	Validada por uma expressão regular* do tipo: correspondente à:

	​``` 
	^([1-9]{1}[\d]{0,2}(\.[\d]{2})*(\.[\d]{0,2})?|[1-9]{1}[\d]{0,}(\.[\d]{0,2})?|0(\.[\d]{0,2})?|(\.[\d]{1,2})?)$
	​```
	
	Mensagem de aviso em caso de erro: Utilize o padrão 00.00

[![valor](imagens/Recibos/cr_valor.png)](imagens/Recibos/cr_valor.png)

A expressão regular deste campo me permite digitar numeros de até duas casas decimais separados por ponto.

### E-mail
Campo para o E-mail da pessoa que receberá o recibo.

!!! note  ""
	Titulo do campo: E-mail.

	Tipo de campo: Resposta curta

	Descrição: Insira o e-mail para o qual o recibo será enviado.

	Validada por uma expressão regular* do tipo: correspondente à:

	​``` 
	[a-zA-Z0-9_\.\+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-\.]+
	​```
	
	Mensagem de aviso em caso de erro: Digite um e-mail válido.

[![email](imagens/Recibos/cr_email.png)](imagens/Recibos/cr_email.png)

## A Planilha Google

A planilha de respostas deve ser vinculáda ao formulário! Para fazer isso vá na aba respostas do seu form e clique nos tres pontinhos mostrado na figura abaixo.

[![planilhaform](imagens/Recibos/cr_planilhaform.png)](imagens/Recibos/cr_planilhaform.png)

Um menu irá aparecer, selecione a opção "Selecionar destino da resposta". Uma janela semelhante amostrada a baixo apareceu em sua tela crie uma nova planilha ou selecione uma já existente (aconselho criar uma nova planilha).

[![planilhamenu](imagens/Recibos/cr_planilhamenu.png)](imagens/Recibos/cr_planilhamenu.png)

A planilha criada (ou a aba da nova planilha) deve ser assim:

[![planilhacriada](imagens/Recibos/cr_planilhacriada.png)](imagens/Recibos/cr_planilhacriada.png)

### Renomeie a pasta de trabalho!
Troque o nome da sua pasta de trabalho para "Recibos", para fazer isso clique com botão direito sobre o nome da pasta de trabalho e vá em renomear.

[![renomearplanilha](imagens/Recibos/cr_renomearplanilha.png)](imagens/Recibos/cr_renomearplanilha.png)

### Campo ID
Vá insira uma nova coluna a esquerda da coluna "A" (clique na celula "A" com botão direito e vá em incerir coluna à esquerda)

[![campoID](imagens/Recibos/cr_campoID.png)](imagens/Recibos/cr_campoID.png)


Na nova A1 agora vazia escreva a palavra "ID". Sua nova planilha deve ficar assim:

[![resultadocampoID](imagens/Recibos/resultadocampoID.png)](imagens/Recibos/resultadocampoID.png)

### Data e Hora
Crie uma nova coluna C de forma que a coluna B seja o "Carimbo de data/hora" e a coluna "D" seja a "Digite a senha de acesso:" na selula B1 digite a segunte formula:

[![novacolunaC](imagens/Recibos/cr_novacolunac.png)](imagens/Recibos/cr_novacolunac.png)

``` 
=arrayFormula(SE(LIN(INDIRETO("B1:B" & TEXTO(CONT.VALORES(B:B);"#")))=1;"Data e Hora";TEXTO(DIA(B1:B);"##")&" de "& TEXTO(B1:B;"MMMM")&" de " & ANO(B1:B)&" às "&  TEXTO(HORA(B1:B);"00")&"h"&TEXTO(MINUTO(B1:B);"00")))
```

Essa formula é responsabel por retirar pegar da Data da coluna B e transformar ela num texto com o seguinte formato: "1 de janeiro de 1900 às 00h00."

### E-Mail
O campo E-mail deve estar agora na coluna J. Você vai incerir 4 novas colunas a esqueda deste campo fazendo assim que seu campo e-mail vá para coluna "N".

[![cr_novocampoemail](imagens/Recibos/cr_novocampoemail.png)](imagens/Recibos/cr_novocampoemail.png)

### Tipo de Transação
Na Coluna "J" (que deveria estar vazia) vá até a celula K1 e digite: 

``` 
=arrayFormula(IF(ROW(INDIRECT("A1:A" & TEXT(COUNTA(A:A);"#")))=1;"Tipo de transação";"Entrada"))
```

Assim para cada valor linha que for preenchida na planilha voce irá dizer que é uma "Entrada de Caixa"

### Valor pago em Reais

Na celula K1 digete a seguinte formula, (a coluna L deve estar vazia)

``` 
=query(A:K; "select I where I is not null format I 'R$#####0.00' ";1)
```

Ela formata os valores de I (Compo "Valor pago em Reais:" do formulário) para formato monetário.

### Ano
Na Celula L1 digite a formula abaixo para extrair o ANO em que foi emitido o recibo.

``` 
=arrayFormula(IF(ROW(INDIRECT("B1:B" & TEXT(COUNTA(B:B);"#")))=1;"Ano";YEAR(B1:B)))
```

### Mes
Na Celula M1 digite a formula abaixo para extrair o MES em que foi emitido o recibo.

``` 
=arrayFormula(SE(LIN(INDIRETO("B1:B" & TEXTO(CONT.VALORES(B:B);"#")))=1;"Mês";TEXTO(B1:B;"MM")))
```

### Pasta Administrativa
Crie uma nova Pasta de trabalho e troque seu nome para "Pasta adm". Nesta pasta vá ate A1 e digite Unidade, de A2 até A9 o nome das unidades (Ramo, AESS, CS, CPMT, EMBS, PES, RAS ,TEMS) na celula C1 digite: "Nº de Recibos +1" em C2 copie a seguinte formula:

``` 
=CONT.SES(Recibos!F:F;A2;Recibos!L:L;"=2017")+1
```

O valor retornado será o número de recibos já emitidos somado a 1. Veja o Exemplo abaixo: Atente-se ao detalhe que que a coluna B está oculta!

[![pastaAdm](imagens/Recibos/cr_pastaAdm.png)](imagens/Recibos/cr_pastaAdm.png)

## Doc

## Script


### Função right(valor)

```js
String.prototype.right = function() {
    return this.substr(this.length - (arguments[0] == undefined ? 1 : parseInt(arguments[0])), this.length);
}
```

Ela serve para extrair as x últimas letras de uma String.

No exemplo aplicado abaixo a saida é "plo". 
```js
var texto = exemplo
texto.right(3);
```

### Funçao pad(x,y)
```js
function pad(number, length) {
    var str = '' + number;
    while (str.length < length) {
        str = '0' + str;
    }
    return str;
}
```

Ela serve para escrever um número sempre utilizando um numero x de casas.

No exemplo aplicado abaixo a saida é "0006". 

```js
pad(6,4) 
```

### Funçao Extenso(x)

Está função foi desenvolvida pelo Marcelo Camargo e copiada deste [link](https://productforums.google.com/forum/?nomobile=true#!topic/docs-pt/5dZ-BxqBoP4;context-place=topicsearchin/docs-pt/EXTENSO%7Csort:date)


??? note "Abra para ver o código da função"
    ```js
    function Extenso(n, moeda, moedas, centavo, centavos) {
    var j, x, m, r, ri, rd, d, i, casas, erro;
    var v1 = 0,
        v2 = 0,
        v3 = 0,
        v4 = 0,
        v5 = 0,
        v6 = 0;
    r = "";
    rd = "";
    ri = "";
    i = parseInt(n);
    d = n - i;
    d = d.toFixed(2);
    d = d * 100;
    d = d.toFixed(0);
    casas = i.toString().length;

    if (n == "?") {
        return "Função Extenso() Marcelo Camargo - marcelocamargo@gmail.com";
    }
    if (n < 0) {
        return "Erro: número negativo";
    }
    if (moeda != null) {
        if (moedas == null || centavo == null || centavos == null || moeda == "" || moedas == "" || centavo == "" || centavos == "") {
            return "Erro: parâmetros de moeda";
        }
    }

    if (d == 100) {
        d = 0;
        i = i + 1;
    }

    if (casas > 12) {
        v5 = (parseInt(i / 1000000000000) * 1000000000000 - parseInt(i / 1000000000000000) * 1000000000000000) / 1000000000000;
        if (v5 > 0) {
            j = "";
            x = CentenaExtenso(v5);
            if (v5 > 1) {
                ri = ri + j + x + " trilhões";
            } else {
                ri = ri + j + x + " trilhão";
            }
        }
    }
    if (casas > 9) {
        v4 = (parseInt(i / 1000000000) * 1000000000 - parseInt(i / 1000000000000) * 1000000000000) / 1000000000;
        if (v4 > 0) {
            if (v5) {
                j = ", ";
            } else {
                j = "";
            }
            x = CentenaExtenso(v4);
            if (v4 > 1) {
                ri = ri + j + x + " bilhões";
            } else {
                ri = ri + j + x + " bilhão";
            }
        }
    }
    if (casas > 6) {
        v3 = (parseInt(i / 1000000) * 1000000 - parseInt(i / 1000000000) * 1000000000) / 1000000;
        if (v3 > 0) {
            if (v4 + v5) {
                j = ", ";
            } else {
                j = "";
            }
            x = CentenaExtenso(v3);
            if (v3 > 1) {
                ri = ri + j + x + " milhões";
            } else {
                ri = ri + j + x + " milhão";
            }
        }
    }
    if (casas > 3) {
        v2 = (parseInt(i / 1000) * 1000 - parseInt(i / 1000000) * 1000000) / 1000;
        if (v2 > 0) {
            if (v3 + v4 + v5) {
                j = ", ";
            } else {
                j = "";
            }
            x = CentenaExtenso(v2);
            if (v2 == 1) {
                ri = ri + j + "mil";
            } else {
                ri = ri + j + x + " mil";
            }
        }
    }
    if (casas > 0) {
        v1 = (parseInt(i).toFixed(0)) - (parseInt(i / 1000).toFixed(0) * 1000);
        if (v1 > 0) {
            if (v2 + v3 + v4 + v5) {
                if (v1 <= 100) {
                    j = " e ";
                } else {
                    j = ", ";
                }
            } else {
                j = "";
            }
            x = CentenaExtenso(v1);
            ri = ri + j + x;
        }
    }

    if (moeda == null) {
        moedas = "reais";
        moeda = "real";
        centavos = "centavos";
        centavo = "centavo";
    }
    if ((d != 0 && moeda == "inteiro") || moeda != "inteiro") {
        if (i > 0 && !v1) {
            ri = ri + " de " + moedas;
        } else if (i > 1 && v1 == 1) {
            ri = ri + " " + moedas;
        } else if (v1 == 1) {
            ri = ri + " " + moeda;
        } else if (v1 > 1) {
            ri = ri + " " + moedas;
        } else if (i == 1) {
            ri = ri + " " + moeda;
        }
    }

    if (d == 1) {
        rd = "um " + centavo;
    } else if (d > 1 && d < 100) {
        rd = CentenaExtenso(d) + " " + centavos;
    }
    if (i < 1 && d > 0 && moeda != "inteiro") {
        rd = rd + " de " + moeda;
    } else if (i == 0 && d == 0) {
        rd = "zero " + moeda;
    }

    if (d > 0 && i > 0) {
        rd = " e " + rd;
    }

    r = ri + rd;
    return r;

    }

    function CentenaExtenso(n) {
        var u, d, c, casas;
        var r = "";
        var t1 = ["um", "dois", "três", "quatro", "cinco", "seis", "sete", "oito", "nove"];
        var t2 = ["dez", "onze", "doze", "treze", "quatorze", "quinze", "dezesseis", "dezessete", "dezoito", "dezenove"];
        var t3 = ["vinte", "trinta", "quarenta", "cinquenta", "sessenta", "setenta", "oitenta", "noventa"];
        var t4 = ["cento", "duzentos", "trezentos", "quatrocentos", "quinhentos", "seiscentos", "setecentos", "oitocentos", "novecentos"];
        casas = n.toString().length;
        u = 0;
        d = 0;
        c = 0;
        if (n > 0) {
            u = parseInt(n.toString().substr(casas - 1, 1));
        }
        if (n > 9) {
            d = parseInt(n.toString().substr(casas - 2, 1));
        }
        if (n > 99) {
            c = parseInt(n.toString().substr(casas - 3, 1));
        }
        if (n == 100) {
            return "cem";
        } else {
            if (c > 0) {
                r = r + t4[c - 1];
                if (d > 0 || u > 0) {
                    r = r + " e ";
                }
            }
            if (d > 1) {
                r = r + t3[d - 2];
                if (u > 0) {
                    r = r + " e ";
                }
            } else if (d == 1 && u >= 0) {
                r = r + t2[d + u - 1];
            }
            if (u > 0 && d != 1) {
                r = r + t1[u - 1];
            }
        }
        return r;
    }
    ```


Ela serve para escrever um número por extenso.

No exemplo aplicado abaixo a saida é "cento e cinco". 
``` js
Extenso(105)
```

### Função Recibo

Está função é a função que trata os dados do formulário para gerar o recibo e enviar suas copias para o destinatário e para uma pasta do driver. Por ela ser uma funçao muito grande irei fragmenta-lá e explicar a passo como construir ela.

??? note "Abra para ver o código da função completo"
    ``` js
    function send_Rec_Email() {
        // ID do modelo recibo no Google Docs
        var recibotemplateId = "14Zlj5zwYyWHhAUnBG9dMFYTzeClIa_xvayxJsB8k_Os"
        
        // Carrega planilha ativa
        var ss = App.getActive()
        //var ss = App.getActive().getSheetByName("Recibos")
        
        var dados = ss.getDataRange().getValues();
        var ultimaLinha = ss.getLastRow() - 1; //Pega a última linha da tabela
        
        // Defino a pasta 2 da minha planilha como ativa
        var sheet = App.openById(submissionSSKey).getSheets()[2];
        
        // informações da planilha
      // var datarecibo = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o data do recibo , lembrando que a coluna A vale 0(zero)'];
        var datarecibo = dados[ultimaLinha][2];

        // var evento = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o valor do curso, lembrando que a coluna A vale 0(zero)'];
        var evento = dados[ultimaLinha][4];
        
        // var unidade = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o unidade responsavel, lembrando que a coluna A vale 0(zero)'];
        var unidade = dados[ultimaLinha][5];
        
        // var nome_completo = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o Nome completo da pessoa, lembrando que a coluna A vale 0(zero)'];
        var nome_completo = dados[ultimaLinha][6];
        
        // var CPF = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o CPF da pessoa, lembrando que a coluna A vale 0(zero)'];
        var CPF = String(dados[ultimaLinha][7]);
        
        // Formata o CPF no formato de 123.456.789-11
        if (CPF.lenght != 11) {
            CPF = '0'.concat(CPF);
        }
        var CPF = CPF.replace(/^(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");
        
        // var valor = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o valor do curso, lembrando que a coluna A vale 0(zero)'];
        var valor = dados[ultimaLinha][10];
        
        // var valor = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o valor do curso, lembrando que a coluna A vale 0(zero)'];
        var valorextenso = Extenso(valor);
        
        // var ano = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o Ano da transaçao no formato de 4 numeros, lembrando que a coluna A vale 0(zero)'];
        var ano = dados[ultimaLinha][11];
        
        // var mes = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o mes da transaçao no formato de 2 numeros, lembrando que a coluna A vale 0(zero)'];
        var mes = dados[ultimaLinha][12];
        
        //var destinatariorecibo = dados[ultimaLinha]['coloque aqui o nº da coluna onde ficara o e-mail, lembrando que a coluna A vale 0(zero)'];
        var destinatariorecibo = dados[ultimaLinha][13];
        
        // Teste id recibo
        var range;
        // Define um ID para cada unidade.
        var idunidade;
        // Busca na planilha uma célula específica (neste caso o número do recibo).
        // range = sheet.getRange(linha,coluna);
        if (unidade == "Ramo") {
            range = sheet.getRange(2, 3);
            idunidade = "00";
        } else if (unidade == "AESS") {
            range = sheet.getRange(3, 3);
            idunidade = "01";
        } else if (unidade == "CS") {
            range = sheet.getRange(4, 3);
            idunidade = "02";
        } else if (unidade == "CPMT") {
            range = sheet.getRange(5, 3);
            idunidade = "03";
        } else if (unidade == "EMBS") {
            range = sheet.getRange(6, 3);
            idunidade = "04";
        } else if (unidade == "PES") {
            range = sheet.getRange(7, 3);
            idunidade = "05";
        } else if (unidade == "RAS") {
            range = sheet.getRange(8, 3);
            idunidade = "06";
        } else if (unidade == "TEMS") {
            range = sheet.getRange(9, 3);
            idunidade = "07";
        }
         
        // Defino o número do recibo com 4 algarismos.
        var numrecibo = pad(range.getValue(), 4);
        
        // Chama a função "right" para pegar os 2 últimos digitos da data preenchido no formulário.
        var ano = ano.toString().right(2);
        
        // Monta o ID do Recibo
        var idrecibo = idunidade + mes + numrecibo + ano + "ER";
        
        // escreve na planilha "Recibos" o ID do recibo
        ss.getSheets()[0].getRange(ultimaLinha+1, 1).setValue(idrecibo);
        
        // defino destinatario do Canhoto - Fixo 
        var destinatariocanhoto = "adson.batista@live.com";
        
        //EMAIL
        // Assunto do email
        var subject = "Recibo IEEE UFABC";
        
        // Mensagem do Corpo 
        var html =
            '<body>' +
            '<h2><b>Olá ' + nome_completo + '!' + '</h2></b>' +
            'Você está recebendo este e-mail pois no dia ' + '<b>' + datarecibo + '</b>' +
            ' você efetuou um pagamento no valor de <b> ' + valor + ' (' + valorextenso + ') ' + '</b>' + 'referente ao ' + '<b>' + evento + '</b>' + '<br>' +
            'Seu recibo foi anexado neste email e pode ser identificado pelo ID' + '<b>' + idrecibo + '</b>' + '.' +
            '</body>'
        
        // Dados do Rementente
        var remetente = "IEEE UFABC<contato@ieeeufabc.org>";
        
        // Cria um recibo temporário, recupera o ID e o abre
        var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(idrecibo).getId();
        
        // var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(recibotempDoc +'_' + id_recibo + '_' + nome_completo).getId();
        var docCopia = DocumentApp.openById(idCopia);
        
        // recupera o corpo do recibo
        var bodyCopia = docCopia.getActiveSection();
        
        // faz o replace das variáveis do template, salva e fecha o documento temporario
        bodyCopia.replaceText("NOME", nome_completo);
        bodyCopia.replaceText("NUMEROCPF", CPF);
        bodyCopia.replaceText("VALOR", valor);
        bodyCopia.replaceText("VALEXTENSO", valorextenso);
        bodyCopia.replaceText("CURSO", evento);
        bodyCopia.replaceText("DATARECIBO", datarecibo);
        bodyCopia.replaceText("IDRECIBO", idrecibo);
        docCopia.saveAndClose();
        
        // abre o documento temporario como PDF utilizando o seu ID
        var recibo_pdf = DriveApp.getFileById(idCopia).getAs("application/pdf");
        
        //Pastas Drive para Salvar recibos
        var folderramoID = "0B8CcpExpMKFlZElETVFjOGd0elk";
        var folderAESSID = "0B8CcpExpMKFlZElETVFjOGd0elk";
        var folderCSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folderCPMTID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folderEMBSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folderPESID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folderRASID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folderTEMSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
        var folder_recibo_CS = DriveApp.getFolderById(folderCSID);
        var folder_recibo_CPMT = DriveApp.getFolderById(folderCPMTID);
        var folder_recibo_EMBS = DriveApp.getFolderById(folderEMBSID);
        var folder_recibo_PES = DriveApp.getFolderById(folderPESID);
        var folder_recibo_RAS = DriveApp.getFolderById(folderRASID);
        var folder_recibo_TEMS = DriveApp.getFolderById(folderTEMSID);
        var folder_recibo_ramo = DriveApp.getFolderById(folderramoID);
        var folder_recibo_AESS = DriveApp.getFolderById(folderAESSID);
        
        //salva pdf na pasta do ID
        if (unidade == "Ramo") {
            folder_recibo_ramo.createFile(recibo_pdf)
        } else if (unidade == "AESS") {
            folder_recibo_AESS.createFile(recibo_pdf)
        } else if (unidade == "CS") {
            folder_recibo_CS.createFile(recibo_pdf)
        } else if (unidade == "CPMT") {
            folder_recibo_CPMT.createFile(recibo_pdf)
        } else if (unidade == "EMBS") {
            folder_recibo_EMBS.createFile(recibo_pdf)
        } else if (unidade == "PES") {
            folder_recibo_PES.createFile(recibo_pdf)
        } else if (unidade == "RAS") {
            folder_recibo_RAS.createFile(recibo_pdf)
        } else if (unidade == "TEMS") {
            folder_recibo_TEMS.createFile(recibo_pdf)
        }
        
        // envia o email com recibo para destinatario
        // MailApp.sendEmail(destinatariorecibo, subject, body, {name: remetente, attachments: recibo_pdf});
        MailApp.sendEmail(destinatariorecibo, subject, html, {
            name: remetente,
            htmlBody: html,
            attachments: recibo_pdf
        });
        // envia o email recibo para email do ramo  
        MailApp.sendEmail(destinatariocanhoto, subject, html, {
            name: remetente,
            htmlBody: html,
            attachments: recibo_pdf
        });
        
        // apaga o documento temporário
        DriveApp.getFileById(idCopia).setTrashed(true);
    }
    ```

#### Acessando os documentos necessários.

A primeira coisa que devemos fazer em nossa função é definir quais são os documentos que vamos utilizar durante o processo. 

``` js
    var recibotemplateId = "14Zlj5zwYyWHhAUnBG9dMFYTzeClIa_xvayxJsB8k_Os"
    var past_recibos = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Recibos")
    var past_adm = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Pasta adm")
```

Foram criadas 3 variáveis a primeira delas é `recibotemplateId` contém o Id unico de um do meu arquivo de template criado anteriomente. 

As variáveis `plan_recibos` e `plan_adm` pegam a planilha ativa e selecionam respectivamente as pastas de trabalho "Recibos" e "Pasta adm". Ou seja quando eu chamar a variavel `plan_recibos` ou `past_adm` eu estou chamando abrindo a pasta de trabalho correspondente.

!!! note ""
    No nosso caso pegar uma planilha ativa é um metódo funcional, pois o Google Form ativa uma planilha para escrever os dados quando recebe os dados e nos estaremos utilizando justamente essa planilha para trabalhar com os dados. Se desejar buscar a planilha pelo seu Id substitua `getActiveSpreadsheet()` por `openById(Id da planilha)`

    O comando `getSheetByName("Nome da pasta")` poderia ser substituido pelo comando `getSheets()["posição da pasta"]` sendo que a primeira pasta é tem a posição 0

#### Extraindo dados da Pasta Recibos.

Depois de indicar quais serão documentos vamos trabalhar devemos pegar os dados das planilhas para isso utilizamos aos seguintes comandos:

```js
    var dados_recibos = plan_recibos.getDataRange().getValues();
    var ultimaLinha = plan_recibos.getLastRow() - 1;
```

A variavel `dados_recibos` pega toda os valores preenchidos na pasta de trabalho `plan_recibos` essas variáveis são quardadas em vetores numerados em que o vetor "0" armazena os dados da linha 1 da planilha. A variável ultima linha ela nos dá a ultima linha preenchida pasta de trabalho.

!!! important ""
    O comando `getLastRow()` me retorna um número, este numéro indica a ultima linha em branco da tabela de dados! Como este comando retorna um número eu posso fazer operações matemáticas com ele. Então para selecionar a ultima linha preenchida na variável ultima linha eu subtrai "um" deste número.

Ao seguirmos os passos acima transformamos nossa tabela de dados em uma Matriz de dados. Para extrair os dados da nossa matriz utilizamos o comando `matriz de dados[linha][coluna]`em que a celula A1 seria indicada pela linha 0 e coluna 0.

Os comandos abaixo se aplicados em uma planilha como a criada anteriormente guardam em suas variaveis os respectivos dados da ultima linha preenchida: Data do recibo, Evento, Unidade responsável, Nome completo, CPF, Valor pago, Ano da transação, Mês da transaçao e o e-mail registrado no formulário.

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

#### Tratando os dados extraidos

Para melhor efeito visual ou necessidade alguns dados devem ser tratados para isso utilizamos os seguintes algoritimos:

```js
    if (CPF.lenght != 11) {
        CPF = '0'.concat(CPF);
    }
    CPF = CPF.replace(/^(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");
    ano = ano.toString().right(2);
    var valorextenso = Extenso(valor);
```

O primeiro `if` é responsavel por verificar se o CPF é uma string de 11 casas. caso não seja ele adiciona um 0 a esqueda. Posteriormente o CPF é formatado para o formato 123.456.789-11 por meio da expressão regular do algoritmo.

!!! note ""
    O CPF é armazenado como um número na planilha. Alguns CPF's iniciam com 0 então nestes casos o CPF ficaria com 10 algarismos. Para solucionar este problema o algoritmo transforma o CPF numa string verifica se ele tem 11 caracteres se não tiver adiciona um 0 a esqueda.

```js
    ano = ano.toString().right(2);
  
```

A variavel `ano` é um numéro de 4 digitos nete campo ela é transformada numa string e posteriormente dois caracteres da direta são armazenados

```js  
  var valorextenso = Extenso(valor);
```

A variavel `valorextenso` é a string que contém o numéro do campo "valor" escrito na sua forma extensa.


A variavel `idunidade` é escrito a partir do valor extraido e armazenado na variável `unidade`. Para determinar qual valor deve ser escrito é utilizada a seguinte função:

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

Veja que no inicio do algoritimo a variável `idunidade` é definida, mas só tem um valor tribuido quando ela respeita uma das condições de igualdade.

!!! attention ""
    No algoritimo completo mostrado na seção [Função Recibo](#Função Recibo)  o alguritimo acima é utilizado juntamente com o algoritimo da seção [Extraindo dados da Pasta Adm](#Extraindo dados da Pasta Adm)  que é responsavel por definir o valor de da variavel `numrecibo`.

#### Extraindo dados da Pasta Adm

Ao criarmos a Pasta Adm na seção [Pasta Administrativ](#Pasta Administrativ)indicamos qual é número que indica o próximo recibo emitido para pegarmos este numéro utilizamos o seguinte algoritimo:

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

A variável `numrecibo`só tem um valor tribuido quando ela respeita uma das condições de igualdade. Esse valor será igual ao valor extraido pelo comando `getValue` da celula em inidicada pelo comando `getRange(linha,coluna)` , em que o valor a celula A1 seria 1,1, formatado pela função pad (**LINKKKK**) para ter sempre com 4 algoritimos.

!!! attention ""
    No algoritimo completo mostrado na seção [Função Recibo](#Função Recibo) o alguritimo acima é utilizado juntamente com o algoritimo do  [Tratando os dados extraidos](#Tratando os dados extraidos) que é responsavel por definir o valor de da variavel `idunidade`.

#### Definindo e registrando o ID do Recibo

Quando criamos nossa planilha deixamos a coluna "A" definida com ID. Este será preenchido por um conjunto de caracteres respeitando o seguinte código:

``` markdown  
O ID do arquivo é CPmm0000yyTK em que:
CP - Unidade em código numérico
mm - Mês da movimentação
0000 - Numero da movimentação
yy - Ano da movimentação
t - **E**ntrada ou **S**aída
K - **M**ovimentação, **R**ecibo, **T**ransferência
```

A variavel `idrecibo` para este algoritimo é definida definidada pela concatenação de variaveis como é mostrado no algorimo abaixo.

```js
    var idrecibo = idunidade + mes + numrecibo + ano + "ER";
```

O comando `setValue("valor")` mostrado na linha de código abaixo é responsavel por escrever o `idrecibo` na primeira coluna da linha que estamos trabalhando.

```js
   dados_recibos.getRange(ultimaLinha+1, 1).setValue(idrecibo);
```

!!! attention ""
    Comando getRange(linha, coluna) tem a celula "A1" como linha 1 e coluna 1 diferentemente do que foi considerado na seção **Extraindo dados da Pasta Recibos**. A soma de uma unidade (`+1`)   no campo referente a linha é para "corrigir" está caracterisitica.

#### Construindo e Salvando o Recibo

```js
// Cria um recibo temporário, recupera o ID e o abre
    var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(idrecibo).getId();

    // var idCopia = DriveApp.getFileById(recibotemplateId).makeCopy(recibotempDoc +'_' + id_recibo + '_' + nome_completo).getId();
    var docCopia = DocumentApp.openById(idCopia);

    // recupera o corpo do recibo
    var bodyCopia = docCopia.getActiveSection();

    // faz o replace das variáveis do template, salva e fecha o documento temporario
    bodyCopia.replaceText("NOME", nome_completo);
    bodyCopia.replaceText("NUMEROCPF", CPF);
    bodyCopia.replaceText("VALOR", valor);
    bodyCopia.replaceText("VALEXTENSO", valorextenso);
    bodyCopia.replaceText("CURSO", evento);
    bodyCopia.replaceText("DATARECIBO", datarecibo);
    bodyCopia.replaceText("IDRECIBO", idrecibo);
    docCopia.saveAndClose();

    // abre o documento temporario como PDF utilizando o seu ID
    var recibo_pdf = DriveApp.getFileById(idCopia).getAs("application/pdf");
```

#### Salvando Recibo no Driver
```js
    //Pastas Drive para Salvar recibos
    var folderramoID = "0B8CcpExpMKFlZElETVFjOGd0elk";
    var folderAESSID = "0B8CcpExpMKFlZElETVFjOGd0elk";
    var folderCSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folderCPMTID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folderEMBSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folderPESID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folderRASID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folderTEMSID = "0B8CcpExpMKFlZElETVFjOGd0elk"
    var folder_recibo_CS = DriveApp.getFolderById(folderCSID);
    var folder_recibo_CPMT = DriveApp.getFolderById(folderCPMTID);
    var folder_recibo_EMBS = DriveApp.getFolderById(folderEMBSID);
    var folder_recibo_PES = DriveApp.getFolderById(folderPESID);
    var folder_recibo_RAS = DriveApp.getFolderById(folderRASID);
    var folder_recibo_TEMS = DriveApp.getFolderById(folderTEMSID);
    var folder_recibo_ramo = DriveApp.getFolderById(folderramoID);
    var folder_recibo_AESS = DriveApp.getFolderById(folderAESSID);

    //salva pdf na pasta do ID
    if (unidade == "Ramo") {
        folder_recibo_ramo.createFile(recibo_pdf)
    } else if (unidade == "AESS") {
        folder_recibo_AESS.createFile(recibo_pdf)
    } else if (unidade == "CS") {
        folder_recibo_CS.createFile(recibo_pdf)
    } else if (unidade == "CPMT") {
        folder_recibo_CPMT.createFile(recibo_pdf)
    } else if (unidade == "EMBS") {
        folder_recibo_EMBS.createFile(recibo_pdf)
    } else if (unidade == "PES") {
        folder_recibo_PES.createFile(recibo_pdf)
    } else if (unidade == "RAS") {
        folder_recibo_RAS.createFile(recibo_pdf)
    } else if (unidade == "TEMS") {
        folder_recibo_TEMS.createFile(recibo_pdf)
    }
```

#### Construindo o E-mail
```js
    var subject = "Recibo IEEE UFABC";
    var html =
        '<body>' +
        '<h2><b>Olá ' + nome_completo + '!' + '</h2></b>' +
        'Você está recebendo este e-mail pois no dia ' + '<b>' + datarecibo + '</b>' +
        ' você efetuou um pagamento no valor de <b> ' + valor + ' (' + valorextenso + ') ' + '</b>' + 'referente ao ' + '<b>' + evento + '</b>' + '<br>' +
        'Seu recibo foi anexado neste email e pode ser identificado pelo ID' + '<b>' + idrecibo + '</b>' + '.' +
        '</body>'
    var remetente = "IEEE UFABC<contato@ieeeufabc.org>";
``` 