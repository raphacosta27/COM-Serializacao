---
title: Camada Física -  APS 9 - Serialização/Desserialização 
author: Elisa Malzoni e Raphael Costa - elisamm@al.insper.edu.br e raphaelamc1@al.insper.edu.br
date: Novembro - 2017
---
# Projeto 9 - Serialização/Desserialização
## Comunicação UART

  A UART é um dispositivo de comunicação assíncrona através da serialização de dados, no qual o formato do dado e a velocidade de transmissão podem ser configuráveis. O data framing baseia-se nos seguintes passos: primeiro, é enviado constantemente um bit 1 (High) antes que ocorra qualquer transmissão de dados. Assim, ao iniciar a transmissão, é mandado um bit 0 (Low) nomeado Start Bit, para inidicar o inicio da transmissão. Após, são enviados os bits dos dados normalmente. Após o termino, é enviado um ou mais stop bits para indicar o fim da transmissão deste byte.
  
## Exibir a forma de onda gerada pela implementação (usando o analog discovery)

A mensagem a ser transmitida era: `Cam Fisica Rapha e Elisa`, assim a forma de onda gerada pelo envio dos três primeiros caracteres é representado na imagem abaixo.

![analog](analog.jpeg)

## Explicar o código

O código funciona da seguinte forma: existem três arquivos para cada ponto da comunicação, 3 para o Transmissor e 3 para o Receptor. Os arquivos .ino chamam as funções feitas nos arquivos .cpp e mandam para o arduino. Ja os arquivos .h por sua vez  que tornam possível a chamada das funções do cpp no arquivo ino.
Para o Transmissor, a função sw_uart_write_byte é a principal e é a que descreve a lógica de escrita dos bits serializados. Basicamente, cada bit é escrito através da função digitalWrite(). Primeiramente, escreve-se o start bit e é feito um for com 8 iterações, escrevendo cada bit de um char a partir do bit menos significativo (bit a esquerda); este processo é efetuado a partir de duas operações: n Shifts de bits a direita, sendo n a posição do bit a ser enviado; e um and lógico. Ao final deste laço, manda-se o bit de paridade, que é calculado através da função calc_even_parirty(), e o stop bit, completando o envio de um char. Este processo repete-se infinitamente devido ao laço que o arduino efetua em sua função void loop().
	No Receptor, a função sw_uart_receive_byte é quem recebe os bits e é quem descreve a lógica de desserialização dos bits recebidos. Cada bit é recebido através da função digitalRead(). Primeiramente, o código entra em um laço do tipo while até que seja recebido um bit 0 (Low) que indica um start bit. Assim, ao recebê-lo inicia-se o recebimento dos outros bits em um laço do tipo for com 8 iterações também, escrevendo cada bit recebido em uma variável do tipo char; este processo é efetuado com duas operações que se opõem as operações de escrita: n Shifts a esquerda, sendo n a posição do bit a ser gravado; e um or lógico.
