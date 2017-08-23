Este é um Google Script voltado para emissão de recibos gerais do Ramo Estudantil IEEE UFABC. Ele faz parte de um processo de automatização financeira que está sendo desenvolvido.

Suas funções são:

* Receber os dados do formulário e organiza-los numa planilha.
* Cria uma movimentação e ID unico para ela. 
* Cria um Recibo em pdf e renomeia utilizando o ID.
* Salva o Recibo no driver do ramo em uma pasta já definida previamente.
* Envia um e-mail para a pessoa efetuou  o pagamento com o recibo em PDF e com o link do driver caso ela prefira.

## Estrutura do Projeto
Para o devido funcionamento deste *script* é necessárias 8 pastas que serão utilizadas para gravar os recibos de 3 arquivos que são:

 * Uma "Formulário Google". - Ele servirá de entrada de dados do sistema é o único "arquivo" que o usuário final terá acesso.
 * Um "Documentos Google". - Ele é o modelo utilizado para gerar o recibo propriamente dito.
 * Uma "Planilha Google" - Ela é seu banco de dados, aqui serão depositadas todas as respostas do seu formulário, é onde serão feitas algumas operações necessárias e o *script* que fará as funções mais avançadas é criado nesta planilha.
     * Um Script. - Ele é criado diretamente na planilha, ele é responsável pelas interações entre APIs do Google, e ele é composto por 3 arquivos:
         * rec_email_template.html - É o modelo utilizado para gerar o e-mail enviado.
         * recibo.gs:  Contém a rotina principal do script.
         * auxiliar.gs: Contém as funções auxiliares utilizadas. 

## Implementação do Projeto

Sua implementação foi dividida nas seguintes etapas que serão exploradas nas proximas páginas:

* [Criação do formulário](recibos_m_form.md)
* [Criação do modelo de recibo](recibos_m_docs.md)
* Manipulações com Planilha
  * [Criação das pastas de trabalho](recibos_m_planilha.md)
  * [Produção do Script](recibos_m_planilha_estr.md)
    * [Arquivo Auxiliar.gs](recibos_m_planilha_aux.md)
    * [Arquivo rec_mail_template.html](recibos_m_planilha_html.md)
    * [Arquivo recibo 1.0](recibos_m_planilha_rec1.md)
    * [Arquivo recibo 2.0:](recibos_m_planilha_rec2.md)

## Manutenção do Projeto

Uma vez implementado existem alguns pontos que caso dê algum problema pode-se identificar facilmente a região responsavel.

* Adicionar campos ao formuário
* Dados inceridos errados no modelo
* Não envio do E-mail
* Falhas visuais no email
* Não Salvar o recibo no driver