# Ambiente local: Docker + WSL2 + Airflow + SQL Server

No trabalho eu preciso testar DAGs do Airflow que leem e gravam num banco SQL Server, sem depender do ambiente de produção a cada tentativa. Esse repositório documenta como montei esse ambiente do zero, num notebook Windows, sem custo de licença.

<img width="3200" height="1800" alt="Jornada WSL2 Docker-selection" src="https://github.com/user-attachments/assets/35533a64-996d-43f2-85f4-29173f6ddb34" />

## Stack

WSL2 com Ubuntu, Docker Engine (sem Docker Desktop), Airflow 3.1.1 com CeleryExecutor, SQL Server 2019 em container (edição gratuita para dev), Postgres para os metadados do Airflow e Redis como fila.

## Por que essas escolhas

Docker Engine em vez de Docker Desktop: gratuito pra qualquer uso e mais leve, sem interface gráfica.

Containers Linux: as imagens do Airflow e afins são feitas pra Linux. Não tem alternativa Windows equivalente.

## Estrutura

`docs/`: decisões técnicas e o passo a passo.
`docker/`: arquivos de composição dos containers.
`sql/`: scripts de inicialização do banco, sem dados reais.

## Status

Em andamento. Acompanho o progresso em `docs/`.
