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


