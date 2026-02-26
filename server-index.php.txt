<?php

function Convert($amount, $from, $to){

    $fileXML="valute.xml";
    $xml = simplexml_load_file($fileXML);

    $tassi = [];

    foreach($xml->valuta as $v) {
        $tassi[(string)$v['codice']] = (float)$v['fattore'];
    }

    if(isset($tassi[$from]) && isset($tassi[$to])){
        $ris = round(($amount / $tassi[$from] * $tassi[$to]), 2);
        return $ris;
    }

    return "Valuta non valida";
}

$server = new SoapServer("funzione.wsdl");
$server->addFunction("Convert");
$server->handle();

?>