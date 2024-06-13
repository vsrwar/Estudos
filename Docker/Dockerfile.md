📘 [Documentação](https://docs.docker.com/reference/dockerfile/) 

O Dockerfile nada mais é do que **um meio que utilizamos para criar nossas próprias imagens**. Em outras palavras, ele serve como a receita para construir um container, permitindo definir um ambiente personalizado e próprio para meu projeto pessoal ou empresarial.

#### Comandos
| Instrução                                                                           | Descrição                                                            |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| [`ADD`](https://docs.docker.com/reference/dockerfile/#add)                          | Adicione arquivos e diretórios locais ou remotos.                    |
| [`ARG`](https://docs.docker.com/reference/dockerfile/#arg)                          | Use variáveis ​​de tempo de construção.                              |
| [`CMD`](https://docs.docker.com/reference/dockerfile/#cmd)                          | Especifique comandos padrão.                                         |
| [`COPY`](https://docs.docker.com/reference/dockerfile/#copy)                        | Copie arquivos e diretórios.                                         |
| [`ENTRYPOINT`](https://docs.docker.com/reference/dockerfile/#entrypoint)            | Especifique o executável padrão.                                     |
| [`ENV`](https://docs.docker.com/reference/dockerfile/#env)                          | Defina variáveis ​​de ambiente.                                      |
| [`EXPOSE`](https://docs.docker.com/reference/dockerfile/#expose)                    | Descreva em quais portas seu aplicativo está escutando.              |
| [`FROM`](https://docs.docker.com/reference/dockerfile/#from)                        | Crie um novo estágio de construção a partir de uma imagem base.      |
| [`HEALTHCHECK`](https://docs.docker.com/reference/dockerfile/#healthcheck)          | Verifique a integridade de um contêiner na inicialização.            |
| [`LABEL`](https://docs.docker.com/reference/dockerfile/#label)                      | Adicione metadados a uma imagem.                                     |
| [`MAINTAINER`](https://docs.docker.com/reference/dockerfile/#maintainer-deprecated) | Especifique o autor de uma imagem.                                   |
| [`ONBUILD`](https://docs.docker.com/reference/dockerfile/#onbuild)                  | Especifique instruções para quando a imagem for usada em um build.   |
| [`RUN`](https://docs.docker.com/reference/dockerfile/#run)                          | Execute comandos de construção.                                      |
| [`SHELL`](https://docs.docker.com/reference/dockerfile/#shell)                      | Defina o shell padrão de uma imagem.                                 |
| [`STOPSIGNAL`](https://docs.docker.com/reference/dockerfile/#stopsignal)            | Especifique o sinal de chamada do sistema para sair de um contêiner. |
| [`USER`](https://docs.docker.com/reference/dockerfile/#user)                        | Defina o ID do usuário e do grupo.                                   |
| [`VOLUME`](https://docs.docker.com/reference/dockerfile/#volume)                    | Crie montagens de volume.                                            |
| [`WORKDIR`](https://docs.docker.com/reference/dockerfile/#workdir)                  | Altere o diretório de trabalho.                                      |

#### Exemplo
```
FROM mcr.microsoft.com/dotnet/sdk:<version> AS build-env
WORKDIR /app
USER dotnet

COPY WP.Core ./WP.Core
COPY WP.Dashboard.API ./WP.Dashboard.API
WORKDIR ./WP.Dashboard.API
RUN dotnet restore WP.Dashboard.csproj
RUN dotnet publish WP.Dashboard.csproj -c Release -o ../out

FROM mcr.microsoft.com/dotnet/aspnet:<version> AS runtime
WORKDIR /app
COPY --from=build-env /app/out .
COPY --from=build-env /app/out/*.json /usr/share/dotnet
ENTRYPOINT ["dotnet", "WP.Dashboard.dll"]
```
