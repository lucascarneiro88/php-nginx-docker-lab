# Ambientes Docker com PHP, MySQL e NGINX configurados para estudos e experimentos.


## 📦 Dockerfile Personalizado para PHP

Este projeto contém um Dockerfile baseado na imagem oficial `php:8.3.13-fpm-alpine`, customizado para incluir bibliotecas e extensões essenciais para o desenvolvimento de aplicações PHP modernas.

### 🔧 O que foi adicionado

- Instalação de bibliotecas nativas necessárias:
  - `libpng-dev`, `libjpeg-turbo-dev`, `freetype-dev` – suporte a imagens (GD)
  - `libzip-dev` – suporte a arquivos ZIP
  - `icu-dev`, `libxml2-dev`, `openssl-dev` – compatibilidade geral com extensões PHP
  - `bash`, `git`, `unzip` – ferramentas auxiliares
  - Pacotes de build: `gcc`, `g++`, `make`

- Instalação de extensões PHP:
  - `gd`
  - `pdo`
  - `pdo_mysql`

- Instalação do Composer:
  - Baixado diretamente do site oficial e instalado em `/usr/local/bin/composer`

### 🐳 Como construir a imagem

No diretório `php/`, execute o comando:

```bash
docker build -t php:teste .
```


![Docker Build Output](/img/img_build_docker.png)

-----------

# Configuração Nginx

Este arquivo `default.conf` configura o servidor Nginx para rodar uma aplicação PHP com PHP-FPM.

## Principais configurações

- **Porta:** O servidor escuta na porta `80`.
- **Root:** Define a pasta `/var/www/html/public` como diretório raiz, onde ficam os arquivos públicos do projeto.
- **Index:** O Nginx procura pelos arquivos `index.php`, `index.html` ou `index.htm` ao acessar a raiz.
- **Regras para rotas:**
  - Usa `try_files` para servir arquivos estáticos diretamente.
  - Caso o arquivo não exista, redireciona a requisição para `index.php` com a query string, habilitando URLs amigáveis.
- **PHP-FPM:**
  - Arquivos `.php` são processados pelo PHP-FPM, que deve estar disponível no container `php` na porta `9000`.
  - Ajusta o parâmetro `SCRIPT_FILENAME` para apontar ao arquivo PHP dentro da pasta pública.
- **Segurança:** Bloqueia o acesso a arquivos ocultos (ex: `.git`, `.env`).

## Observações

- Garanta que o container PHP esteja na mesma rede Docker com o nome `php` para o `fastcgi_pass` funcionar.
- A pasta pública do projeto deve estar montada no caminho `/var/www/html/public` dentro do container Nginx.


# Instruções básicas para subir o ambiente com Docker Compose

No terminal, dentro da pasta do projeto, execute:

```bash
docker-compose up -d
```

![Docker compose up](/img/img_compose.png)

Este comando foi utilizado para:

- Criar uma rede Docker chamada `php_network` para comunicação entre os containers.
- Construir a imagem personalizada do PHP com extensões necessárias instaladas.
- Baixar as imagens oficiais do Nginx e do MySQL, se ainda não estivessem presentes localmente.
- Criar e iniciar os containers do PHP, Nginx e MySQL em modo destacável (`-d`), ou seja, em segundo plano.

### O que aconteceu:

- O Docker construiu a imagem do PHP a partir do `Dockerfile` especificado.
- O Nginx e o MySQL foram baixados da Docker Hub, se necessário.
- Os containers foram criados com os nomes `php`, `nginx` e `mysql`.
- A rede `php_network` foi configurada para permitir comunicação entre eles.

---

##  Verificação dos Containers em Execução

Com o comando `docker ps`, foi possível confirmar que os containers estavam rodando corretamente, apresentando:

- O container `nginx` escutando na porta 80 do host, permitindo acesso HTTP.
- O container PHP disponível na porta 9000 para processamento PHP-FPM.
- O container MySQL exposto na porta padrão 3306, para conexões do banco de dados.

---

##  Mudanças na Estrutura do Projeto

Após a inicialização dos containers, a listagem dos arquivos mostra que os diretórios `volume_app` e `volume_data` foram criados automaticamente pelo Docker. Isso indica que:

- O Docker criou os volumes para armazenar dados persistentes da aplicação PHP (`volume_app`).
- O volume para os dados do banco MySQL (`volume_data`) também foi criado, garantindo que os dados do banco sejam preservados entre reinícios.

---
---------------------------------

# 🛠️ Recriação da Imagem PHP com Nome Personalizado

A imagem Docker do serviço PHP foi **recriada com um nome personalizado** (`php:8.3.13_personal`) para facilitar a organização e identificação no projeto.


Anteriormente, o Docker gerava um nome automático para a imagem (`php_nginx_mysql_php:latest`), o que pode causar confusão em projetos maiores ou em ambientes compartilhados.  
Por isso, foi removida a imagem antiga com o comando:

```bash
docker rmi php_nginx_mysql_php:latest
```


E em seguida, no docker-compose.yml, foi definido um nome específico para a imagem:




```bash
image: php:8.3.13_personal
```

![Docker novo vome img container php](/img/img_novo_nome.png)