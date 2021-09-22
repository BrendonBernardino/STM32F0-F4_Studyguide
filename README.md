# STM32F0-F4_Studyguide

## CLOCKS:

> LSI, LSE, HSI, HSE, PLL

LSI - é um clock direcionado para o Watchdog e para o RTC(mas sem precisão). 32768 Khz.
Quando realizamos o cálculo para ter a resolução do clock do RTC em 1 seg = 1Hz, obtemos 32768Khz/32Khz = 1024, ou seja, não resolve para 1 seg = 1 Hz.

LSE - é um clock mais preciso para o RTC, vem de um crystal externo de 32 Khz. Ideal para o RTC.

Obs: O RTC só pode usar as configurações de LSI, LSE e HSE.

HSI - é o clock interno do processador. 16 Mhz. Esse clock é usado para o System Clock, ou seja, é o clock que vai para os periféricos.

HSE - é outro clock externo do processador, esse de 8 Mhz - 26 Mhz, também pode ser usado para o SysClk.

PLL - é uma forma de alcançar clocks maiores, é um multiplicador de frequência de clock.

Optando por configurar o RTC em HSE com 8Mhz, temos que configurar um divisor de 8, para que passe 1Mhz pro RTC e seja possível uma resolução de 1 seg no RTC.
