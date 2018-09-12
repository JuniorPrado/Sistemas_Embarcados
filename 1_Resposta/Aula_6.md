

 1- Como se utiliza o comando ps para:

(a) Mostrar todos os processos rodando na máquina?

```C
ps

```

(b) Mostrar os processos de um usuário?

```C
ps -u nome_do_usuario
```
(c) Ordenar todos os processos de acordo com o uso da CPU?

```C
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head

```


(d) Mostrar a quanto tempo cada processo está rodando?

```C
ps -e -o pid,ppid,comm,etime

```


3- De onde vem o nome fork()?

Bifurcação

4- Quais são as vantagens e desvantagens em utilizar:

(a) system()?

(b) fork() e exec()?

5- É possível utilizar o exec() sem executar o fork() antes?

Não, porque não tem como criar um processo e executa-lo ao mesmo tempo, por isso utilizamos o fork para
criar um processo e o exec para executar.
6- Quais são as características básicas das seguintes funções:

(a) execp()?

(b) execv()?

(c) exece()?

(d) execvp()?

(e) execve()?

(f) execle()?
