index.php
```html
<!doctype html>
<html>

<head>
    <script src="//code.jquery.com/jquery-3.0.0.min.js"></script>
    <script>
        $(document).ready(function () {
            // Requisicao AJAX
            var requisicao = function () {
                $.ajax({
                    url: "contagem.php"
                }).done(function (resultado) {
                    // Exibe o resultado da contagem.php no elemento com ID contador
                    $("#contador").html(resultado);
                });
            };
            // Executa a requisicao com intervalo de 1 segundo
            setInterval(requisicao, 1000);
        });
    </script>
</head>

<body>
    <!-- Exibe o resultado da resposta ajax no elemento com ID contador -->
    <p>Faltam <span id="contador"></span></p>
</body>

</html> 
```

contagem.php
```php
<?php

// Sua função de cancelar a promoção
function cancelaPromo() {
    return null;
}


// Marca o horario local
date_default_timezone_set('America/Sao_Paulo');
// Informe a data e horario do termino do evento
$dia_hora_evento = strtotime(date("10-06-2016 12:32:00"));

// Calcula o tempo atual
$dia_hora_atual = time();

// Calcula a diferença entre as datas...
$diferenca = $dia_hora_evento - $dia_hora_atual;

// Inicia as variaveis com zero
$dias = 0;
$hora = 0;
$minuto = 0;
$segundos = 0;

// Calcula a contagem
if($diferenca>=0){
    $dias = intval($diferenca / 86400);
    $marcador = $diferenca % 86400;
    $hora = intval($marcador / 3600);
    $marcador = $marcador % 3600;
    $minuto = intval($marcador / 60);
    $segundos = $marcador % 60;
}
else
    cancelaPromo();

// Molda os resultados obtidos para enviar ao ajax
$retorno = "$dias dia(s) $hora hora(s) $minuto minuto(s) $segundos segundo(s)";

// Marca o header da requisição
header('Content-Type: application/json');

// Retorna a resposta para a captura da requisição ajax
echo json_encode($retorno); 
```
