# Docker Compose Laravel
Um docker compose que cria um ambiente de containers para um desenvolvimento local de um projeto laravel. 

Repositório criado baseado neste [artigo](https://dev.to/aschmelyun/the-beauty-of-docker-for-local-laravel-development-13c0).

## Clonando ou criando seu projeto Laravel

Clone seu projeto ou copie todos os arquivos diretamente dentro desta pasta `src`. 

Ou crie um novo projeto Laravel executando o comando abaixo em seu terminal, será gerado pelo composer um projeto laravel na pasta `src`.
```
docker-compose run --rm composer create-project laravel/laravel .
``` 

## Executando o servidor web

Na pasta raíz, execute os containers do servidor web com o comando `docker-compose up -d --build site`.
Ao rodar a rede do Docker Compose com o `site` em vez de usar `up`, executa apenas os containers do site. 
O servidor web é montado, com as seguintes portas expostas:

- **nginx** - `:8080`
- **mysql** - `:3306`
- **php** - `:9000`

Três containers adicionais são incluidos para controlar os comandos composer, NPM, e Artisan *sem* ter os programas instalados em sua máquina. Use os seguintes comandos exemplos da root do seu projeto, modificando eles para cada caso específico.

```
docker-compose run --rm composer update
```
```
docker-compose run --rm npm run dev
```
```
docker-compose run --rm artisan migrate`
```

## Manter os dados do MySQL

Por padrão, quando você derruba a rede Docker, seus dados MySQL são removidos depois que seus containers são destruidos. Se você gostaria de manter os dados que restam após os container serem derrubados e trazidos de volta, faça o seguinte:

1. Crie uma pasta `mysql` na raiz do projeto, ao lado das pastas `nginx` e `src`.
2. No arquivo `docker-compose.yml` embaixo do `mysql:` em `services:`, adicione as seguintes linhas:

```
volumes:
  - ./mysql:/var/lib/mysql
```