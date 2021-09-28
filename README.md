# STM32F0-F4_Studyguide

## CLOCKS (Clock Tree): 
#### Entender como configurar devidamente cada clock é essencial para o desenvolvimento de qualquer firmware.
> LSI, LSE, HSI, HSE, PLL

LSI (Low-Speed Internal clock) - é um clock interno do processador, direcionado para o Watchdog e para o RTC(mas sem precisão para problemas que necessitem da precisaão do relógio). 32768 Khz.

Quando realizamos o cálculo* para ter a resolução do clock do RTC em 1 seg = 1Hz, obtemos 32768 Khz / 32 Khz = 1024, ou seja, não resolve para 1 seg = 1 Hz.

*Comentarei sobre o cálculo de prescalers posteriormente.

LSE (Low-Speed External clock) - é um clock externo vindo de um crystal de 32 Khz. Ideal para o RTC, por sua maior precisão.

> Obs: O RTC só pode usar as configurações de LSI, LSE e HSE.

HSI - é o clock interno do processador. 16 Mhz. Esse clock é usado para o System Clock (SysClk), ou seja, é o clock que vai para os periféricos. Importante ressaltar que a configuração do SysClk pode vir do HSE ou PLL, não necessariamente virá do HSI.

HSE - é outro clock externo do processador, esse de 8 Mhz ou até 26 Mhz, também pode ser usado para o SysClk.

> Optando por configurar o RTC em HSE com 8Mhz, temos que configurar um divisor de 8, para que passe 1Mhz pro RTC e seja possível uma resolução de 1 seg no RTC.

PLL - é uma forma de alcançar clocks maiores, é um multiplicador de frequência de clock. Também pode ser usado para o SysClk.

## UART:
Vamos supor que você tenha um dispositivo que se comunica através da UART, o dispositivo envia um conjunto de caracteres a cada X segundos. A primeira coisa que se deve pensar é: Como receber esse conjunto de caracteres para que eu possa tratá-lo?
Bom, existem 3 modos de Receber e Transmitir uma mensagem via UART. 

> Polling, Interrupt, DMA

Primeiro tentaremos receber.

#### Polling

No modo polling o processador fica em modo de espera até que a função seja executada.

HAL_UART_Receive(&huart, [buffer], [tamanhodobuffer], [timeout]);

HAL_UART_Transmit(&huart, [buffer], [tamanhodobuffer], [timeout]);

#### Interrupt

No modo Interrupt, a função roda em paralelo à função principal do processador. Quando o tamanho do buffer é atingido, o callback é acionado - callback é uma função que é chamada toda vez que a interrupção é ativada, no caso ela é ativada quando o tamanho do buffer é alcançado -. Na função de callback é onde podemos tratar a mensagem que foi recebida ou enviada.

HAL_UART_Receive_IT(&huart, [buffer], [tamanhodobuffer]);

HAL_UART_Transmit_IT(&huart, [buffer], [tamanhodobuffer]);

Função que deve ser declarada por você:

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}

void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}

#### DMA

No modo DMA, temos algo similar ao modo Interrupt. A função também roda em paralelo ao processador, pois usa o DMA (estudaremos o DMA mais a frente). Então existe um callback também quando o tamanho do buffer é alcançado, mas além disso, nesse modo também existe um callback no caso da metade do buffer ser alcançada.

HAL_UART_Receive_DMA(&huart, [buffer], [tamanhodobuffer]);

HAL_UART_Transmit_DMA(&huart, [buffer], [tamanhodobuffer]);

Função que deve ser declarada por você:

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}

void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}

void HAL_UART_RxHalfCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}

void HAL_UART_TxHalfCpltCallback(UART_HandleTypeDef *huart) {
  /*Tratamento*/
}
