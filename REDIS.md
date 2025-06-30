## Conexão (via redis-cli)

```bash
# Conectar a um host/porta local (padrão)
redis-cli

# Conectar a um host/porta específico
redis-cli -h <host> -p <port>

# Conectar com senha
redis-cli -a <password>
```

## Comandos Essenciais (Gerenciamento de Chaves)

| Comando                   | Descrição                                                                                                           |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------ |
| `KEYS padrao*`            | Busca chaves que correspondem a um padrão. **CUIDADO:** Lento em produção.                                          |
| `DEL chave [chave ...]`   | Apaga uma ou mais chaves.                                                                                           |
| `EXISTS chave`            | Verifica se uma chave existe. Retorna `1` se sim, `0` se não.                                                       |
| `EXPIRE chave segundos`   | Define um tempo de expiração (em segundos) para uma chave.                                                          |
| `TTL chave`               | Retorna o tempo de vida restante de uma chave (em segundos). `-1` se não tem expiração, `-2` se a chave não existe. |
| `TYPE chave`              | Retorna o tipo de dado armazenado na chave (`string`, `list`, `hash`, etc.).                                        |
| `RENAME chave nova_chave` | Renomeia uma chave.                                                                                                 |
| `RANDOMKEY`               | Retorna uma chave aleatória do banco de dados.                                                                      |

---

## Tipos de Dados e Comandos

### 1. Strings

O tipo de dado mais básico.

| Comando                   | Descrição                                          |
| :------------------------ | :------------------------------------------------- |
| `SET chave valor`         | Define o valor de uma chave.                       |
| `GET chave`               | Obtém o valor de uma chave.                        |
| `GETSET chave novo_valor` | Define um novo valor e retorna o valor antigo.     |
| `MSET chave1 valor1 ...`  | Define múltiplas chaves e valores de uma vez.      |
| `MGET chave1 chave2 ...`  | Obtém os valores de múltiplas chaves.              |
| `INCR chave`              | Incrementa o valor numérico de uma chave em 1.     |
| `DECR chave`              | Decrementa o valor numérico de uma chave em 1.     |
| `APPEND chave valor`      | Adiciona o valor ao final de uma string existente. |

### 2. Lists

Uma lista de strings, ordenada pela inserção.

| Comando                   | Descrição                                                                                             |
| :------------------------ | :---------------------------------------------------------------------------------------------------- |
| `LPUSH chave valor ...`   | Adiciona um ou mais valores no início (esquerda) da lista.                                            |
| `RPUSH chave valor ...`   | Adiciona um ou mais valores no final (direita) da lista.                                              |
| `LPOP chave`              | Remove e retorna o primeiro elemento da lista.                                                        |
| `RPOP chave`              | Remove e retorna o último elemento da lista.                                                          |
| `LLEN chave`              | Retorna o tamanho (número de elementos) da lista.                                                     |
| `LRANGE chave inicio fim` | Retorna um intervalo de elementos da lista. Ex: `LRANGE minha_lista 0 -1` retorna todos os elementos. |
| `LINDEX chave indice`     | Retorna o elemento em um índice específico.                                                           |

### 3. Hashes

Estruturas para armazenar mapas de campos e valores.

| Comando                          | Descrição                                     |
| :------------------------------- | :-------------------------------------------- |
| `HSET chave campo valor`         | Define o valor de um campo dentro de um hash. |
| `HGET chave campo`               | Obtém o valor de um campo de um hash.         |
| `HGETALL chave`                  | Obtém todos os campos e valores de um hash.   |
| `HMSET chave c1 v1 c2 v2 ...`    | Define múltiplos campos e valores de uma vez. |
| `HDEL chave campo ...`           | Apaga um ou mais campos de um hash.           |
| `HKEYS chave`                    | Retorna todos os campos (chaves) de um hash.  |
| `HVALS chave`                    | Retorna todos os valores de um hash.          |
| `HINCRBY chave campo incremento` | Incrementa o valor numérico de um campo.      |

### 4. Sets

Uma coleção de strings únicas e não ordenadas.

| Comando                  | Descrição                                           |
| :----------------------- | :-------------------------------------------------- |
| `SADD chave membro ...`  | Adiciona um ou mais membros a um set.               |
| `SREM chave membro ...`  | Remove um ou mais membros de um set.                |
| `SMEMBERS chave`         | Retorna todos os membros de um set.                 |
| `SISMEMBER chave membro` | Verifica se um membro existe no set.                |
| `SCARD chave`            | Retorna o número de membros (cardinalidade) do set. |
| `SUNION chave1 chave2`   | Retorna a união de dois ou mais sets.               |
| `SINTER chave1 chave2`   | Retorna a interseção de dois ou mais sets.          |

### 5. Sorted Sets (ZSETs)

Similar aos Sets, mas cada membro está associado a um score (pontuação), usado para ordenação.

| Comando                                   | Descrição                                                                          |
| :---------------------------------------- | :--------------------------------------------------------------------------------- |
| `ZADD chave score membro ...`             | Adiciona um ou mais membros com seus scores.                                       |
| `ZRANGE chave inicio fim [WITHSCORES]`    | Retorna um intervalo de membros por índice, ordenados do menor para o maior score. |
| `ZREVRANGE chave inicio fim [WITHSCORES]` | Retorna um intervalo de membros, ordenados do maior para o menor score.            |
| `ZRANGEBYSCORE chave min max`             | Retorna membros dentro de um intervalo de scores.                                  |
| `ZCARD chave`                             | Retorna o número de membros do sorted set.                                         |
| `ZSCORE chave membro`                     | Retorna o score de um membro específico.                                           |
| `ZREM chave membro ...`                   | Remove um ou mais membros do sorted set.                                           |

---

## Transações

| Comando           | Descrição                                                                                                                          |
| :---------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| `MULTI`           | Inicia um bloco de transação. Os comandos seguintes são enfileirados.                                                              |
| `EXEC`            | Executa todos os comandos na fila da transação.                                                                                    |
| `DISCARD`         | Descarta a fila de comandos da transação.                                                                                          |
| `WATCH chave ...` | "Observa" uma chave para execução condicional (CAS - Check-And-Set). Se a chave for modificada antes do `EXEC`, a transação falha. |

---

## Publicar / Subscrever (Pub/Sub)

| Comando                       | Descrição                                                       |
| :---------------------------- | :-------------------------------------------------------------- |
| `SUBSCRIBE canal [canal ...]` | Inscreve o cliente em um ou mais canais para receber mensagens. |
| `PUBLISH canal mensagem`      | Publica uma mensagem em um canal.                               |
| `UNSUBSCRIBE [canal ...]`     | Cancela a inscrição em canais.                                  |

---

## Servidor

| Comando        | Descrição                                                       |
| :------------- | :-------------------------------------------------------------- |
| `PING`         | Verifica se o servidor está respondendo. Retorna `PONG`.        |
| `INFO [secao]` | Retorna informações e estatísticas sobre o servidor.            |
| `FLUSHDB`      | **CUIDADO:** Apaga TODAS as chaves do banco de dados atual.     |
| `FLUSHALL`     | **CUIDADO:** Apaga TODAS as chaves de TODOS os bancos de dados. |
| `SAVE`         | Força o salvamento do dataset no disco (síncrono).              |
| `BGSAVE`       | Salva o dataset no disco em background (assíncrono).            |
