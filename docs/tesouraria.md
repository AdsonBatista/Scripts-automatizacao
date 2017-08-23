O projeto de automatização da tesouraria tem como intuito manter em um unico local de forma organizada todos os dados de tesouraria para o Ramo Estudantil IEEE UFABC. 
Isso consiste em manter um registro unico e padronizado para os recibos e movimentações e a criaçao de um sistema visualização do fluxo de caixa.

Este sistema é composto por:

* Um conjunto de pastas para armazenamento dos recibos digitais, 
* Dois formulários que alimentam uma planilha e as pastas.
* Um modelo de recibo.
* Uma planilha que tem como função auxiliar é armazenar os registros e criar uma estrutura de fluxo de caixa e do ramo e dos capitulos. 
* Google Scripts e GoogleWebApps fazem o processo acontecer.

Este projeto pode ser dividio em 4 projetos menores cuja melhor ordem de construção é a sequencia abaixo: 

## Recibos 
Este é Google Script que emissão de Recibos gerais do Ramo Estudantil IEEE UFABC.
O usuário preenche uma formulário e ele armazena dados na planilha de caixa, envia um email com o recibo em PDF para o destinatairio e grava uma segunda via numa pasta do Driver do Ramo. 

## Movimentações
Este é um GoogleApp **em desenvolvimento**!
O usuário preenche uma formulário, em que deve anexar uma cópia digital do comprovante de movimentação. O WebApp armazena dados na planilha de caixa o comprovante de movimentação vai para o driver e um e-mail é enviado com o resumo da transacão para as pessoas responsáveis. 

## Planilha de caixa
Este é um projeto **em desenvolvimento**!
Está é uma Planilha google responsavel por receber os dados dos formulários e de trata-los!

## Dashbord
Este é um projeto **em idealização**!
É uma forma visual da pessoa acessar os dados uteis da planilha sem ter que abrir a planilha.
