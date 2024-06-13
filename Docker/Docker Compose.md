💻 [Site](https://docs.docker.com/compose/) | 📘 [Documentação](https://docs.docker.com/compose/compose-file/) | ⚙️ [Instalação](https://docs.docker.com/compose/install/)

Docker Compose se baseia em um arquivo de configuração YAML, normalmente nomeado `compose.yaml`.

## Comandos

**Executa os containers** definidos no `compose.yaml`.
```
docker compose up
```
	Parâmetros:
		-d -> Roda em modo detach, libera o terminal.
		--build -> Força o build das imagens antes de subir os containers.
		--remove-orphans -> Remove os containers orfãos.
		--env-file <.env-file> -> Especifica o arquivo das variáveis de ambiente.

**Remove os containers**, networks e volumes definidos no `compose.yaml`.
```
docker compose down
```

**Lista os arquivos compose** que estão rodando
```
docker compose ls
```

**Lista os containers** que estão rodando
```
docker compose ps
```
	Parâmetros:
		-a -> Lista todos os containers (inclusive os parados).

**Visualiza o compose com as variáveis de ambiente aplicadas**
```
docker compose config
```


## compose.yml

#### Services

Define os serviços / containers que deverão subir.
```
services:
    banco-de-dados:
        container_name: <name>
        image: <image>
        restart: always
        environment:
            USERNAME: ${DB_USER:-user}
            PASSWORD: ${DB_USER:-Pwd123}
        ports:
            - <host_port>:<container_port>
    backend:
        image: vsrwar/minha-imagem:${TAG:-latest}
        container_name: meu-container
        build:
            context: .
            dockerfile: dockerfile
        restart: always | on-failure:3 | unless-stopped
        environment:
            ASPNETCORE_ENVIRONMENT: Development
        networks:
            - net
        ports:
            - <host_port>:<container_port>
        depends_on:
            db:
                condition: service_healthy
        command: /bin/bash
        entrypoint: dotnet api.dll
        tty: true
        healthcheck:
            test: ["ping", "http://www.google.com"]
            interval: 10s # Tempo entre uma tentativa e outra
            timeout: 5s # Tempo de espera do test
            retries: 5 # tentativas
            start_period: 40s # delay / tolerância antes de começarem os testes
            start_interval: 5s # delay para o interval
        cpuset="<number>"
        memswap_limit=<quantity>
        deploy:
            resources:
                limits:
                    cpus: "<quantity>"
                    memory: "<quantity>"
    frontend:
        [ ... ]
```

#### Networks
Define as networks
```
networks:
    net:
        name: net
        driver: bridge
    local:
        name: local
        driver: host
```

#### Volumes
Define os volumes
```
volumes:
    meu-volume-01:
        driver: local
    meu-volume-02:
        driver: local
        external: true # se foi criado fora do escopo do compose.yml
```
## Exemplos

Exemplo:
```
services:
	<service_name>:
		container_name: <container_name>
		image: <image>:<tag>
		restart: "no" | always | on-failure | unless-stopped
		environment:
			<VARIABLE_NAME>: <value>
			#- <VARIABLE_NAME>=<value>
		ports:
			- "<local_port>:<container_port>"
```

Exemplo de **build**
```
services:
	<service_name>:
		image: <image>:<tag>
		container_name: <container_name>
		build:
			context: .
			dockerfile: <dockerfile>
		restart: "no" | always | on-failure | unless-stopped
		ports:
			- "<local_port>:<container_port>"
```

Exemplo de network:
```
services:
	<service_name>:
		[...]
		networks:
			- <net_name>

networks:
	<net_name>:
		name: <net_name>
		driver: bridge
```

Exemplo **Real**
```
services:
  db:
    container_name: mongodb
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: Fato123
    ports:
      - 27017:27017
  gateway:
    image: webportal/gateway:${TAG:-release3}
    container_name: gateway
    build:
      context: .
      dockerfile: dockerfile-gateway
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8010:8010"
  dashboard:
    image: webportal/dashboard:${TAG:-release3}
    container_name: dashboard
    build:
      context: .
      dockerfile: dockerfile-dashboard
    restart: always
    environment:
        ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8012:8012"
    depends_on:
      - gateway
      - db
  scheduler:
    image: webportal/scheduler:${TAG:-release3}
    container_name: scheduler
    build:
      context: .
      dockerfile: dockerfile-scheduler
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8016:8016"
    depends_on:
      - gateway
      - db
  recon:
    image: webportal/recon:${TAG:-release3}
    container_name: recon
    build:
      context: .
      dockerfile: dockerfile-recon
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8014:8014"
    depends_on:
      - gateway
      - db
  datawarehouse:
    image: webportal/datawarehouse:${TAG:-release3}
    container_name: dw
    build:
      context: .
      dockerfile: dockerfile-datawarehouse
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8018:8018"
    depends_on:
      - gateway
      - db
  dataviewer:
    image: webportal/dataviewer:${TAG:-release3}
    container_name: dataviewer
    build:
      context: .
      dockerfile: dockerfile-dataviewer
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8020:8020"
    depends_on:
      - gateway
      - db
  messenger:
    image: webportal/messenger:${TAG:-release3}
    container_name: messenger
    build:
      context: .
      dockerfile: dockerfile-messenger
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8022:8022"
    depends_on:
      - gateway
      - db
  datamask:
    image: webportal/datamask:${TAG:-release3}
    container_name: datamask
    build:
      context: .
      dockerfile: dockerfile-datamask
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8026:8026"
    depends_on:
      - gateway
      - db
  externalsystems:
    image: webportal/externalsystems:${TAG:-release3}
    container_name: externalsystems
    build:
      context: .
      dockerfile: dockerfile-externalsystems
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8028:8028"
    depends_on:
      - gateway
      - db
  logger:
    image: webportal/logger:${TAG:-release3}
    container_name: logger
    build:
      context: .
      dockerfile: dockerfile-logger
    restart: always
    environment:
      ASPNETCORE_ENVIRONMENT: Docker
    ports:
      - "8030:8030"
    depends_on:
      - gateway
      - db
  flow:
    image: webportal/flow-10:${TAG:-release3}
    container_name: flow
    build:
      context: WP.Flow/flow-10
      dockerfile: Dockerfile
    restart: always
    ports:
      - "8000:8000"
  frontend:
    image: webportal/frontend:${TAG:-release3}
    container_name: frontend
    build:
      context: .
      dockerfile: dockerfile-frontend
    restart: always
    ports:
      - "31031:80"
    depends_on:
      - gateway
```