

1 - Quantos pipes serão criados após as linhas de código a seguir? Por quê?

(a)
```C
int pid;
int fd[2];
pipe(fd);
pid = fork();
```
Neste código como o PIPE é criado antes do Fork este pipe é compartilhado entre os processos criados pelo FORK. Totalizando um.




(b)

int pid;
int fd[2];
pid = fork();
pipe(fd);

Neste código como o PIPE é criado depois do Fork este PIPE é duplicado um para cada processo. Totalizando dois.



2 - Apresente mais cinco sinais importantes do ambiente Unix, além do SIGSEGV, SIGUSR1, SIGUSR2, SIGALRM e SIGINT. 
    Quais são suas características e utilidades?

  SIGHUP - Sinal emitido aos processos associados quando este se desconecta.
  SIGQUIT - Sinal emitido aos processos associados quando a tecla QUIT ou CTRL + D é pressionada.
  SIGILL - Sinal emitido aos processos associados quando uma instrução ilegal é detectada,
  SIGBUS - Sinal emitido em caso de erro de barramento.
  SIGCLD - Enviado ao pai peça terminação do processo de um filho.

3 - Considere o código a seguir:

#include <signal.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

void tratamento_alarme(int sig)
{
	system("date");
	alarm(1);
}

int main()
{
	signal(SIGALRM, tratamento_alarme);
	alarm(1);
	printf("Aperte CTRL+C para acabar:\n");
	while(1);
	return 0;
}

Sabendo que a função alarm() tem como entrada a quantidade de segundos para terminar a contagem, quão precisos são os alarmes 
criados neste código? De onde vem a imprecisão? Este é um método confiável para desenvolver aplicações em tempo real?








