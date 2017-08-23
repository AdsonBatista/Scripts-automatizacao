## Estrutura do Projeto
Para o devido funcionamento deste *script* é necessárias 8 pastas que serão utilizadas para gravar os recibos de 3 arquivos que são:

 * Uma "Formulário Google". - Ele servirá de entrada de dados do sistema é o único "arquivo" que o usuário final terá acesso.
 * Um "Documentos Google". - Ele é o modelo utilizado para gerar o recibo propriamente dito.
 * Uma "Planilha Google" - Ela é seu banco de dados, aqui serão depositadas todas as respostas do seu formulário, é onde serão feitas algumas operações necessárias e o *script* que fará as funções mais avançadas é criado nesta planilha.
     * Um Script. - Ele é criado diretamente na planilha, ele é responsável pelas interações entre APIs do Google, e ele é composto por 3 arquivos:
         * rec_email_template.html - É o modelo utilizado para gerar o e-mail enviado.
         * recibo.gs:  Contém a rotina principal do script.
         * auxiliar.gs: Contém as funções auxiliares utilizadas. 

