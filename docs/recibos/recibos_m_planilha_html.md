## Script Modelo HTML

Crie um arquivo HTML no *script* utilizando os passos mostrados na seção [Script](#script). Esse arquivo deve ser nomeado como `rec_email_template` e servirá como *template* do e-mail que será enviado! esse novo arquivo é escrito em HTML o modelo utilizado atualmente é:

``` html
<!DOCTYPE html>
<html>
<body>
    <div>
        <h2 class="center-align teal-text">Olá <?= nome_completo ?>!</h2> Você está recebendo este e-mail pois no dia <b> <?= datarecibo  ?></b> você efetuou um pagamento no valor de <b>R$<?= valor ?> (<?= valorextenso ?>)</b> referente a/ao <b><?= evento ?></b>. Seu recibo foi anexado neste email e se necessario pode ser identificado pelo ID: <b> <?=  idrecibo  ?></b>.
    <br>
    Você também pode visualizar seu recibo utilizando a seguinte URL: <br>
    <a href="<?= urlpdf ?>"><?= urlpdf ?></a><br/>
    
    </div>
    <!-- Assinatura  -->
    <br>
    <hr>
    <table>
        <tr>
            <td>
                <img src="https://drive.google.com/uc?id=0B8CcpExpMKFlZXNLdzJKWU9Wcm8" width="150">
            </td>
            <td> 
                <b style="word-space:2em">&nbsp;&nbsp;</b> Site: ieeeufabc.org<br>
                <b style="word-space:2em">&nbsp;&nbsp;</b> Facebook: facebook.com/ieee.ufabc<br>
                <b style="word-space:2em">&nbsp;&nbsp;</b> Twitter: @ieeeufabc<br>
            </td>
        </tr>
    </table>
</body>
</html>
```
