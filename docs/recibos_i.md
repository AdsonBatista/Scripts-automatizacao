Este é um Google Script voltado para emissão de recibos gerais do Ramo Estudantil IEEE UFABC. Ele faz parte de um processo de automatização financeira que está sendo desenvolvido.

Suas funções são:

* Receber os dados do formulário e organiza-los numa planilha.
* Cria uma movimentação e ID unico para ela. 
* Cria um Recibo em pdf e renomeia utilizando o ID.
* Salva o Recibo no driver do ramo em uma pasta já definida previamente.
* Envia um e-mail para a pessoa efetuou  o pagamento com o recibo em PDF e com o link do driver caso ela prefira.