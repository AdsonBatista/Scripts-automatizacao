# Arquivo Auxiliar

Este arquivo é responsável por armazenar todas as funções que não são essencias para as funcionalidades do script. 

## Função formatMoney(casas, separador dec, separado milhar)
```js
Number.prototype.formatMoney = function(c, d, t){
var n = this, 
    c = isNaN(c = Math.abs(c)) ? 2 : c, 
    d = d == undefined ? "." : d, 
    t = t == undefined ? "," : t, 
    s = n < 0 ? "-" : "", 
    i = String(parseInt(n = Math.abs(Number(n) || 0).toFixed(c))), 
    j = (j = i.length) > 3 ? j % 3 : 0;
   return s + (j ? i.substr(0, j) + t : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + t) + (c ? d + Math.abs(n - i).toFixed(c).slice(2) : "");
 };
```

Formata um número para um o formato decimal com número de casas "C", o separador decimal padrão é "." e o separador milhar padrão é ","

No exemplo aplicado abaixo a saída seria 25,50
```js
(25.5).formatMoney(2, ',', '');
```

## Função right(valor)

```js
String.prototype.right = function() {
    return this.substr(this.length - (arguments[0] == undefined ? 1 : parseInt(arguments[0])), this.length);
}
```

Ela serve para extrair as x últimas letras de uma *String*.

No exemplo aplicado abaixo a saída é "plo". 
```js
var texto = exemplo
texto.right(3);
```

## Função pad(x,y)
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

No exemplo aplicado abaixo a saída é "0006". 

```js
pad(6,4) 
```

## Funçao Extenso(x)

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


Ela serve para escrever um número por extenso. O valor de entrada deve obrigatoriamente ser definido como *Number*!

No exemplo aplicado abaixo a saída é "cento e cinco". 
``` js
Extenso(105)
```