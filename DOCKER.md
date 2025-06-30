## Ciclo de Vida do Contêiner (Container Lifecycle)

Comandos para criar, iniciar, parar e remover contêineres.

| Comando | Descrição |
| :--- | :--- |
| `docker run [OPÇÕES] IMAGEM` | Cria e inicia um novo contêiner a partir de uma imagem. |
| `docker start [ID_OU_NOME]` | Inicia um ou mais contêineres que estão parados. |
| `docker stop [ID_OU_NOME]` | Para um ou mais contêineres em execução. |
| `docker restart [ID_OU_NOME]`| Reinicia um contêiner. |
| `docker rm [ID_OU_NOME]` | Remove um ou mais contêineres. Use `-f` para forçar a remoção de um contêiner em execução. |
| `docker exec -it [ID_OU_NOME] /bin/bash` | Executa um comando dentro de um contêiner em execução (ex: abre um terminal interativo). |
| `docker pause [ID_OU_NOME]` | Pausa todos os processos em um contêiner. |
| `docker unpause [ID_OU_NOME]` | Despausa os processos em um contêiner. |

#### Opções comuns para `docker run`:
* `-d`: Modo "detached" (roda em background).
* `-p 8080:80`: Mapeia a porta 8080 do host para a porta 80 do contêiner.
* `--name meu-container`: Define um nome para o contêiner.
* `-v /path/no/host:/path/no/container`: Mapeia um volume (pasta) do host para o contêiner.
* `--rm`: Remove o contêiner automaticamente quando ele para.
* `-e VAR=valor`: Define uma variável de ambiente.

---

## Gerenciamento de Imagens (Image Management)

Comandos para construir, baixar e gerenciar imagens Docker.

| Comando | Descrição |
| :--- | :--- |
| `docker build -t nome:tag .` | Constrói uma imagem a partir de um Dockerfile no diretório atual. |
| `docker pull imagem:tag` | Baixa uma imagem ou um repositório do Docker Hub (ou outro registry). |
| `docker images` ou `docker image ls` | Lista todas as imagens locais. |
| `docker rmi imagem:tag` | Remove uma ou mais imagens. Use `-f` para forçar. |
| `docker login` | Autentica em um registry (ex: Docker Hub). |
| `docker push usuario/imagem:tag` | Envia uma imagem para um registry. |
| `docker tag origem:tag destino:tag`| Cria uma tag para uma imagem. |

---

## Listagem e Informações (Listing & Info)

Comandos para inspecionar contêineres e o sistema.

| Comando | Descrição |
| :--- | :--- |
| `docker ps` | Lista os contêineres em execução. |
| `docker ps -a` | Lista todos os contêineres (em execução e parados). |
| `docker logs [ID_OU_NOME]` | Exibe os logs de um contêiner. Use `-f` para seguir os logs em tempo real. |
| `docker inspect [ID_OU_NOME]` | Exibe informações detalhadas (em JSON) sobre um contêiner ou imagem. |
| `docker top [ID_OU_NOME]` | Exibe os processos em execução dentro de um contêiner. |
| `docker stats` | Exibe o uso de CPU, memória e rede dos contêineres em tempo real. |

---

## Redes (Networking) e Volumes (Data Persistence)

| Comando | Descrição |
| :--- | :--- |
| `docker network ls` | Lista as redes disponíveis. |
| `docker network create nome-da-rede` | Cria uma nova rede bridge. |
| `docker network connect rede container`| Conecta um contêiner a uma rede. |
| `docker network disconnect rede container`| Desconecta um contêiner de uma rede. |
| `docker volume ls` | Lista os volumes. |
| `docker volume create nome-volume` | Cria um novo volume para persistir dados. |
| `docker volume rm nome-volume` | Remove um volume. |
| `docker volume inspect nome-volume`| Exibe informações detalhadas de um volume. |

---

## Limpeza do Sistema (System Cleanup)

**CUIDADO:** Use estes comandos com atenção, pois eles removem dados permanentemente.

| Comando | Descrição |
| :--- | :--- |
| `docker system prune` | Remove todos os contêineres parados, redes não utilizadas, imagens pendentes (dangling) e cache de build. |
| `docker system prune -a` | Remove tudo o que o `prune` normal remove, **mais todas as imagens não utilizadas** por nenhum contêiner. |
| `docker system prune --volumes` | Remove tudo o que o `prune` normal remove, **mais todos os volumes não utilizados**. |
| `docker container prune` | Remove todos os contêineres parados. |
| `docker image prune` | Remove imagens pendentes (dangling). |

---

## Dockerfile - Instruções Essenciais

Um `Dockerfile` é um script com instruções para construir uma imagem Docker.

| Instrução | Descrição |
| :--- | :--- |
| `FROM` | Define a imagem base para a construção. Ex: `FROM node:18-alpine`. |
| `WORKDIR /app` | Define o diretório de trabalho padrão para os comandos seguintes. |
| `COPY . .` | Copia arquivos/pastas do host para o sistema de arquivos do contêiner. |
| `RUN` | Executa um comando na camada da imagem durante a construção. Ex: `RUN npm install`. |
| `CMD ["node", "app.js"]` | Define o comando padrão a ser executado quando o contêiner iniciar. Só pode haver um `CMD`. |
| `EXPOSE 80` | Documenta a porta que o contêiner expõe. Não publica a porta de fato. |
| `ENV NOME=valor` | Define uma variável de ambiente. |
| `ENTRYPOINT` | Configura um contêiner para ser executado como um executável. |

#### Exemplo de `Dockerfile`
```Dockerfile
# Estágio de build
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Estágio de produção
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## Docker Compose - Orquestração Simples

Ferramenta para definir e rodar aplicações com múltiplos contêineres. Usa um arquivo `docker-compose.yml`.

| Comando | Descrição |
| :--- | :--- |
| `docker-compose up` | Cria e inicia os contêineres definidos no `docker-compose.yml`. |
| `docker-compose up -d` | Inicia os contêineres em modo "detached" (background). |
| `docker-compose down` | Para e remove os contêineres, redes e volumes criados pelo `up`. |
| `docker-compose ps` | Lista os contêineres do projeto. |
| `docker-compose logs [serviço]` | Exibe os logs dos serviços. Use `-f` para seguir. |
| `docker-compose build [serviço]`| Constrói ou reconstrói as imagens dos serviços. |
| `docker-compose exec serviço cmd` | Executa um comando em um serviço. Ex: `docker-compose exec web /bin/bash`. |

#### Exemplo de `docker-compose.yml`
```yaml
version: '3.8'

services:
  # Serviço da aplicação web
  webapp:
    build: .
    ports:
      - "8080:80"
    environment:
      - DATABASE_URL=postgres://user:password@db:5432/mydatabase
    depends_on:
      - db

  # Serviço do banco de dados
  db:
    image: postgres:14-alpine
    restart: always
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydatabase
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```
