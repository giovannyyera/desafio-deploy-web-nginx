# Desafio Prático - Deploy de Aplicação Web

Este projeto foi desenvolvido como parte de um desafio prático para uma vaga de Dev Júnior.

O objetivo foi configurar uma aplicação web estática em um servidor web Nginx, utilizando Docker para simular um ambiente Linux local.

## Tecnologias utilizadas

- HTML
- CSS (aplicado diretamente no arquivo HTML por ser uma página estática simples)
- Nginx
- Docker
- Docker Compose
- OpenSSL

## Estrutura do projeto

```text
desafio-deploy-web/
├── docker-compose.yml
├── html/
│   ├── index.html
│   ├── .env.exemplo
│   └── noticias/
│       └── index.html
├── nginx/
│   └── default.conf
├── certs/
│   ├── local.crt
│   └── local.key
├── prints/
└── README.md
```

## Como rodar o projeto 

Para rodar o projeto, é necessário ter o Docker Desktop instalado. 

Acesse a pasta do projeto:

```bash
cd desafio-deploy-web
```

Suba o container com:

```bash
docker compose up -d
```

Acesse no navegador:

```text
http://localhost:8080
```

O acesso HTTP será redirecionado para HTTPS:

```text
https://localhost:8443
```

Como foi utilizado um certificado autoassinado, pode exibir um aviso de segurança, porém é só avançar para continuar em ambiente local.

Para parar o container:

```bash
docker compose down
```

## Testes realizados

Pelo navegador, é possível validar a navegação das páginas, o redirecionamento para HTTPS, o redirecionamento de `/blog` para `/noticias/` e o bloqueio da rota `/admin`.

### Testes pelo navegador

Com o container rodando:

```text
http://localhost:8080
```

O endereço deve redirecionar para:

```text

https://localhost:8443

```

Também é possível testar as rotas:

```text

https://localhost:8443/blog

```

Deve redirecionar para `/noticias/`.

```text

https://localhost:8443/admin

```

Deve retornar acesso bloqueado com erro `403 Forbidden`.

Para testar o bloqueio do arquivo `.env`, pode ser criado localmente um arquivo `html/.env` com valores fictícios, baseado no `.env.exemplo`. Esse arquivo `.env` não é versionado no repositório.

```text

https://localhost:8443/.env

```

Deve retornar acesso bloqueado com erro `403 Forbidden`.

```text

https://localhost:8443/.git/config

```

Deve retornar acesso bloqueado com erro `403 Forbidden`.



## Configurações realizadas

Foi configurado um servidor Nginx para exibir arquivos estáticos da pasta `html/`.

O Docker Compose foi utilizado para subir um container com Nginx, foram mapeadas a porta `8080` para HTTP e a porta `8443` para HTTPS.

Também foram utilizados volumes para conectar os arquivos locais do projeto com as pastas usadas pelo Nginx dentro do container.

Foi adicionada uma regra para redirecionar `/noticias` para `/noticias/`, mantendo a porta `8443` no ambiente local, isso evitou que o navegador tentasse acessar a rota HTTPS sem a porta usada pelo Docker.

A configuração do servidor está no arquivo:

```text
nginx/default.conf
```

Nesse arquivo foram configurados:

- Virtual Host com `localhost` e `meusite.local`

- Redirecionamento de HTTP para HTTPS

- Certificado HTTPS local autoassinado

- Redirecionamento de `/blog` para `/noticias/`

- Bloqueio da rota `/admin`

- Bloqueio de acesso a arquivos `.env`

- Bloqueio de acesso a arquivos `.git`

- Desativação da listagem de diretórios

- Headers básicos de segurança

## Evidências

As evidências de funcionamento estão na pasta `prints/`.

Foram incluídos prints e vídeos curtos mostrando:

- Docker iniciando o container
- Página principal funcionando
- Redirecionamento de `/blog` para `/noticias/`
- Bloqueio da rota `/admin`
- Bloqueio do arquivo `.env`
- Bloqueio da pasta `.git/config`
