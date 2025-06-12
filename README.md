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