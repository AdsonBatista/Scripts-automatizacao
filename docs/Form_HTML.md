Formulários HTML

Durante a construção do script utilizaremos de alguns formulários HTML para construir estes formulários utilizamos os recursos do Materializecss. Toda documentação necessária para fazer estes os formuários encontra-se no site: http://materializecss.com/



Para facilitar a construção de formulários com materializecss pode-se usar a ferramenta: https://forms.studio (veja aqui)

??? note "Abra para ver o código da função completo"
   ```html
      <!DOCTYPE html>
      <html>
         <head>
            <base target="_blank">
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=0.5,maximum-scale=4">
            <!-- Importar as configurações gráficas gerais de repositórios da internet -->
            <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.100.1/css/materialize.min.css">
            <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/sweetalert/1.1.3/sweetalert.css">
            <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
            <!-- Importar configurações CSS do arquivo Stylesheet.html -->
            <?!= include('Stylesheet'); ?>
         </head>
         <body>
            <div class="FormContentWrapper">
               <!-- "HeaderMast" define a região verde-escura do formulário-->
               <div class="HeaderMast"></div>
               <!-- "CenteredContent" define largura da região de escrita e a posição dela-->
               <div class="CenteredContent">
                  <div class="FormCard">
                     <!-- "Accent" linha verde mais clara entre o formulário e o verde escuro-->
                     <div class="Accent"></div>
                     <div class="FormContent">
                        <!-- "Formdiv" é a "div" para ocultar o formulário-->
                        <form novalidate="novalidate" id="forminner" style="display: initial;">
                        <!-- "Accent" linha verde mais clara entre o formulário e o verde escuro-->
                        </form>
                     </div>
                  </div>
                  <div class="col s12" id="success2">
                     <!-- Está div será substituida pelo modelo "obrigado.html" depois da submissão do formulário. -->
                  </div>
               </div>
            </div>
            <!-- Estutura "Modal" utilizada nos botões flutuantes -->
            <div id="modal1" class="modal modal-fixed-footer">
               <div class="modal-content">
                  <h4>Modal Header</h4>
                  <p>A bunch of text</p>
               </div>
               <div class="modal-footer">
                  <a class="modal-action modal-close waves-effect waves-green btn-flat ">Agree</a>
               </div>
            </div>
            <!-- Botões flutuantes -->
            <div class="fixed-action-btn vertical click-to-toggle">
               <a class="btn-floating btn-large red"><i class="material-icons">menu</i></a>
               <ul>
                  <li><a class="btn-floating pink modal-trigger" href="#modal1"><i class="material-icons">help_outline</a></li>
                  <li><a class="btn-floating red"><i class="material-icons">insert_chart</i></a></li>
                  <li><a class="btn-floating yellow darken-1"><i class="material-icons">format_quote</i></a></li>
                  <li><a class="btn-floating green"><i class="material-icons">publish</i></a></li>
                  <li><a class="btn-floating blue"><i class="material-icons">attach_file</i></a></li>
               </ul>
            </div>
            <!-- Importar funções "JavaScript" do arquivo JavaScript.html -->
            <?!= include('JavaScript'); ?>
         </body>
      </html>
   ```








   ```

