
1 - Escreva o texto "Ola mundo cruel!" em um arquivo denominado "Ola_mundo.txt". Apresente o conteúdo deste arquivo no terminal.

   ```C
   echo "Olá Mundo Cruel" >> Ola_Mundo.txt
   nano Ola_Mundo.txt
   ```
   
  2 - Apresente o nome de todos os arquivos e pastas na pasta 'root'.
  
  ```C
   ls /
  ```
   
3 - Apresente o tipo de todos os arquivos e pastas na pasta 'root'.

   ```C
   ls -l /
   
   ```
4 - Apresente somente as pastas dentro da pasta 'root'.

 ```C
   ls -d */
   
   ```
5 - Descubra em que dia da semana caiu o seu aniversário nos últimos dez anos.
```C
date -d "27 mar 2017";date -d "27 mar 2016";date -d "27 mar 2015";
date -d "27 mar 2014";date -d "27 mar 2013";date -d "27 mar 2012";
date -d "27 mar 2011";date -d "27 mar 2010";date -d "27 mar 2009";

```
6 - Liste somente os arquivos com extensão .txt.

```C
ls *.txt

```

7 - Liste somente os arquivos com extensão .png.

```C
ls *.png

```

8 - Liste somente os arquivos com extensão .jpg.

```C
ls *.jpg

```
9 - Liste somente os arquivos com extensão .gif.
```C
ls *.gif

```
10 - Liste somente os arquivos que contenham o nome 'cal'.

```C
ls .| grep "cal"

```
11 - Liste somente os arquivos que contenham o nome 'tux'.
```C
ls .| grep -i "tux"

```
12 - Liste somente os arquivos que comecem com o nome 'tux'.
```C
ls .| grep -i "^tux"

```
