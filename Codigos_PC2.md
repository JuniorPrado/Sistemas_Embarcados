```C
#include <stdio.h>
#include <wiringPi.h>

#define	BUZZER	3
#define MOTOR	2

PI_THREAD (beep)
{
  for (;;)
  {
    digitalWrite (BUZZER, HIGH) ;	// Liga
    delay (100) ;		// 
    digitalWrite (BUZZER, LOW) ;	// Desliga
    delay (100) ;
  }
}

PI_THREAD (vibra)
{
  for (;;)
  {
    digitalWrite (MOTOR, HIGH) ;	// Liga
    delay (100) ;		// 
    digitalWrite (MOTOR, LOW) ;	// Desliga
    delay (100) ;
  }
}

int main (void)
{
  printf ("Raspberry Pi teste\n") ;

  wiringPiSetup () ;
  pinMode (BUZZER, OUTPUT) ;
  pinMode (MOTOR, OUTPUT) ;

  piThreadCreate (beep);
  piThreadCreate (vibra);
  for (;;)
  {
    printf ("Perigo") ;
    delay (600) ;
  }

  return 0 ;
}


```
