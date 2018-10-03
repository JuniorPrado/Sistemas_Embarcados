Para todas as questões, utilize as funções da biblioteca `stdio.h` de leitura e de escrita em arquivo (`fopen()`, `fclose()`, `fwrite()`, `fread()`, dentre outras). Compile os códigos com o gcc e execute-os via terminal.

1. Crie um código em C para escrever "Ola mundo!" em um arquivo chamado 'ola_mundo.txt'.
```C
#include <stdio.h>
#include <stdlib.h> 

int main(int argc, const char * argv[]) {

FILE *fp;
fp=fopen ("ola_mundo.txt","w");
char c[100] = "Olá Mundo cruel!"; 
fputs(c, fp);
return 0;
}

```


2. Crie um código em C que pergunta ao usuário seu nome e sua idade, e escreve este conteúdo em um arquivo com o seu nome e extensão '.txt'. Por exemplo, considerando que o código criado recebeu o nome de 'ola_usuario_1':

```C

#include <stdio.h>
#include <stdlib.h> 

int main(int argc, const char * argv[]) {

FILE *fp;

char name[100];
int age;

fp=fopen ("Antonio.txt","w");
printf ("Digite seu nome: ");
scanf ("%s", name);
printf ("Digite sua idade: ");
scanf ("%d", &age);
fprintf(fp, "Digite o seu nome: %s \n", name);
fprintf(fp, "Digite a sua idade: %d \n", age);

fclose(fp);
return 0;
}
```
```bash
$ cat Antonio.txt 
Nome: Antonio 
Idade: 26 

```


3. Crie um código em C que recebe o nome do usuário e e sua idade como argumentos de entrada (usando as variáveis `argc` e `*argv[]`), e escreve este conteúdo em um arquivo com o seu nome e extensão '.txt'. Por exemplo, considerando que o código criado recebeu o nome de 'ola_usuario_2':

```C
#include <stdio.h>
#include <stdlib.h>


int main(int argc, const char * argv[]) {

	char nome[100];
	int idade;


	FILE *pf;
	pf = fopen ("Antonio_2.txt","w");
	fprintf(pf, "Nome: %s \n", argv[1]);
	fprintf(pf, "Idade: %s \n", argv[2]);

	fclose(pf);
	return 0;
}
```

```bash
$ ./ex3 Antonio 26
cat Antonio_2.txt 
Nome: Antonio 
Idade: 26 

```

