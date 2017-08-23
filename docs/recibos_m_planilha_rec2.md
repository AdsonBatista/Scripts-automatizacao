## Função Recibo 2.0.0

separação em funções....
todo: fazer documentação da versão dois do script
todo: escrever sobre a trigger para que o recibo funcione corretamente a cada submissão.



??? note "Abra para ver o código da função completo"
    ``` js
        function recibo() {
       var past_recibos = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Recibos")

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
       var destinatariorecibo = dados_recibos[ultimaLinha][13]
    
        if (CPF.lenght != 11) {
            CPF = '0'.concat(CPF);
        }
        CPF = CPF.replace(/^(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");
        ano = ano.toString().right(2);
        var valorextenso = Extenso(valor);
        valor = valor.formatMoney(2, ',', '');
        destinatariorecibo = destinatariorecibo.toLowerCase();
    
        var dep_unidades = def_unidades(unidade);
        var idunidade = dep_unidades[0];
        var numrecibo = dep_unidades[1] ;
        var pasta_recibo = dep_unidades[2];
    

        var idrecibo = idunidade + mes + numrecibo + ano + "ER";

        past_recibos.getRange(ultimaLinha + 1, 1).setValue(idrecibo);

        var recibo_pdf = criarrecibo(idrecibo, nome_completo, CPF, valor, valorextenso, evento, datarecibo);


        var pdf=  pasta_recibo.createFile(recibo_pdf);
        var urlarquivo = pdf.setSharing(DriveApp.Access.ANYONE, DriveApp.Permission.VIEW).getUrl();

         email(nome_completo, CPF, valor, valorextenso, evento, datarecibo, idrecibo, destinatariorecibo, recibo_pdf);

    }

    function criarrecibo(idrecibo, nome_completo, CPF, valor, valorextenso, evento, datarecibo) {
        var recibotemplateId = "14Zlj5zwYyWHhAUnBG9dMFYTzeClIa_xvayxJsB8k_Os"
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
        return recibo_pdf
    }

    function email(nome_completo, CPF, valor, valorextenso, evento, datarecibo, idrecibo, destinatariorecibo, recibo_pdf) {
        var html = HtmlService.createTemplateFromFile('rec_email_template');
        html.nome_completo = nome_completo;
        html.datarecibo = datarecibo;
        html.valor = valor;
        html.valorextenso = valorextenso
        html.evento = evento;
        html.idrecibo = idrecibo;
        var htmlBody = html.evaluate().getContent();
        var remetente = "IEEE UFABC<contato@ieeeufabc.org>";
        var assunto = "Recibo IEEE UFABC";
        MailApp.sendEmail(destinatariorecibo, assunto, html, {
            name: remetente,
            htmlBody: htmlBody,
            attachments: recibo_pdf
        });
    }

    function def_unidades(unidade) {
        var past_adm = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Pasta adm")
        var pasta_ramo = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_AESS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_CS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_CPMT = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_EMBS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_PES = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_RAS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");
        var pasta_TEMS = DriveApp.getFolderById("0B8CcpExpMKFlZElETVFjOGd0elk");

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
        return [idunidade, numrecibo, pasta_recibo];
    }
    ```