# Dia 1: WSL2 e Docker Engine do zero

Comecei esse projeto porque preciso testar DAGs do Airflow que leem e gravam
num SQL Server, sem depender do ambiente de produção toda vez que quero
tentar algo. A ideia foi montar isso local, sem gastar nada com licença.

## A primeira decisão: WSL2 em vez de máquina virtual

As imagens do Airflow e do SQL Server que o time usa são feitas pra Linux.
O Windows não tem kernel Linux, então não dá pra rodar container Linux nele
direto. Pensei em algumas saídas: uma máquina virtual completa com Hyper-V,
uma máquina Linux separada, ou o WSL2.

Fui de WSL2. Ele roda um kernel Linux de verdade dentro do Windows,
aproveitando a virtualização que o próprio processador já oferece, sem
precisar de outro computador nem gastar mais memória que o necessário.

## A segunda decisão: Docker Engine, não Docker Desktop

O Docker Desktop cobra licença de empresas maiores. O Docker Engine é
gratuito pra qualquer uso e mais leve, porque não tem interface gráfica nem
máquina virtual própria. Instalei ele direto dentro do Ubuntu, usando o
repositório oficial da Docker (não o que vem padrão no Ubuntu, que costuma
estar desatualizado).

## O que rolou na prática

Instalei o WSL2 com Ubuntu pelo comando `wsl --install -d Ubuntu`. Na
primeira tentativa só o motor (WSL) subiu, o Ubuntu não baixou sozinho.
Rodei o mesmo comando de novo e aí sim ele terminou a instalação e pediu
usuário e senha Linux.

Conferi se o systemd (o programa que liga serviços automaticamente, como o
do Docker) já estava ativo. Já veio habilitado por padrão, não precisei
mexer em nada.

Adicionei o repositório oficial da Docker ao `apt` (o instalador de pacotes
do Ubuntu), com a chave de segurança deles, e instalei:

- `docker-ce` e `docker-ce-cli`: o Docker Engine e a linha de comando
- `containerd.io`: quem de fato roda os containers por baixo
- `docker-buildx-plugin`: pra construir imagens
- `docker-compose-plugin`: o comando `docker compose`, pra subir vários
  containers de uma vez a partir de um arquivo

Coloquei meu usuário no grupo `docker`, pra não precisar de `sudo` toda vez.
Testei com `docker run hello-world`: baixou uma imagem mínima, criou um
container, imprimiu a mensagem de sucesso e terminou sozinho.

## O que aprendi sobre container

Container não é máquina virtual. Máquina virtual simula um computador
inteiro, com kernel próprio, e demora minutos pra ligar. Container
compartilha o kernel do sistema onde roda (aqui, o Linux do WSL2) e só
isola o programa e as bibliotecas dele. Por isso sobe em segundos e usa bem
menos memória.

No nosso caso, cada peça do ambiente (SQL Server, Postgres, Redis, Airflow)
vai virar um container separado, cada um nascendo de uma imagem (o molde
congelado do programa).

## Próximos passos

Organizar as pastas do projeto, subir o SQL Server em container e conectar
nele pelo DBeaver.
