💻 [Site](https://www.docker.com/) | 📘 [Documentação](https://docs.docker.com/) | ⚙️ [Instalação](https://docs.docker.com/engine/install/)

*A criação de containers, volumes e networks pode ser facilitada com o uso do [[Docker Compose]].
## Comandos
#### Containers

**Lista os containers** que estão sendo executados:
```
docker container ls
```
	Parâmetros:
		-a -> Lista todos os containers (inclusive os parados).
		-q -> Lista somente os Ids.

**Executa um container**
```
docker container run <image>
```
	Parâmetros:
		-i -> Modo interativo.
		-t -> Modo TTY, quer permite que o usuário digite via teclado.
		--rm -> Remove o container automáticamente ao encerrar o processo.
		-p <local_port>:<container_port> -> Faz um bind de portas, para que seja               possível acessar o container a partir do host.
		-d -> Roda em modo detach, libera o terminal e imprime o ID do container.
		--name <container_name> -> Nomeia o container.
		-v "<path_local>:<path_container>" -> Mapeia uma pasta local dentro do                 container.
		--network <network_name> -> Especifica em qual network o container vai se              conectar.
		--restart=<on-failure:<times> | unless-stopped | always> -> Define a                   política de rinicialização do container.
		--health-cmd "<healthcheck command>" -> Define um comando para healthcheck.
		--health-timeout <time> -> Tempo de espera por um retorno do --health-cmd.
		--health-retries <times> -> Quantidade de tentativas.
		--health-interval <time> -> Tempo de espera entre uma tentativa e outra.
		--health-start-period <time> -> Tolerância antes de começarem os testes.
		--cpus=<quantity> -> Define o quanto de CPU o container pode usar.
		--cpuset-cpus=<number> -> Define qual CPU processará o container.
		--memory=<quantity> -> Define o quanto de Memoria RAM o container pode                 usar.
		--memory-swap=<quantity> -> Define o quanto de Memoria SWAP o container                pode usar. (Deve ser a soma do parâmetro --memory com a quantidade                  desejada)

**Inicia a execução de um container (existente)**
```
docker container start <container_id | container_name>
```

**Para a execução de um container**
```
docker container stop <container_id | container_name>
```

**Deleta um container**
```
docker container remove <container_id | container_name>
```
	Parâmetros:
		-f -> Força a remoção do container.

**Executa um comando dentro do container**
```
docker container exec <container_id | container_name> <command>
```
	Parâmetros:
		-it -> Executa de modo interativo e com TTY (teclado).

**Vincula o terminal ao container**
```
docker attach <container_name | container_id>
```

**Mostra os logs do container**
```
docker logs <container_name | container_id>
```

#### *Volumes*

**Cria um volume gerenciado pelo Docker**
```
docker volume create <volume_name>
```
	Parâmetros:
		--driver local -> Define o driver como local.

**Lista os volumes**
```
docker volume ls
```

**Detalha um volume**
```
docker volume inspect <volume_name>
```

**Deleta os volumes que não estão sendo utilizados**
```
docker volume prune
```

#### *Imagens*
- São armazenadas em um [[Registry]].
- Podemos criar nossas próprias imagens por meio de um [[Dockerfile]].

**Baixa uma imagem**
```
docker pull <image_name>
```

**Lista as imagens**
```
docker image ls
```

**Deleta uma imagem**
```
docker image remove <image_name:tag | image_id>
```

**Cria uma imagem a partir do [[Dockerfile]]**
```
docker build -t <image_name:tag> <path_to_context>
```
	Parâmetros:
		-f <dockerfile> -> Especifica o dockerfile (caso esteja com outro nome).

**Envia uma imagem para o [[Registry]]**
```
docker push <image_name:tag>
```

**Renomeia uma imagem**
```
docker tag <old_name:tag> <new_name:tag>
```


#### _Networks_

**Cria uma network**
```
docker network create <network_name>
```
	Parâmetros:
		--driver: Especifica o driver (bridge | host).

**Lista as networks**
```
docker network ls
```

**Deleta as networks que não estão em uso**
```
docker network prune
```

**Detalha uma network**
```
docker network inspect <network_name | network_id>
```

**Deleta uma network**
```
docker network remove <network_name | network_id>
```

**Conecta um container à network**
```
docker network connect <network_name> <container_name>
```

#### *Comandos uteis*

**Faz login no [[Registry]]**
```
docker login
```

