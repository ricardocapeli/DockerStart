# Imagens

Antes de de entender o que é imagem é preciso diferenciar container e imagem.

Utilizando uma analogia com orientação a objeto, podemos comparar um container a um objeto (instância),enquanto a imagem seria uma classe (modelo).

Todo container é iniciado a partir de uma imagem, dessa forma podemos concluir que nunca teremos uma imagem em execução.

Um container só pode ser iniciado a partir de uma única imagem. Caso deseje um comportamento diferente, será necessário customizar a imagem.

Toda imagem (bem como os containers) possuem um identificador único em formato hash usando _sha256_. Porém seu uso não é muito prático, então para simplificar isto o docker utiliza uma _tag_ para identificar imagens.

A _tag_ normalmente é formada por um nome, seguido de : dois pontos e depois uma versão.

É extremamente comum utilizar uma versão chamada latest para representar a versão mais atual.

Exemplos de tags de imagens:

* nginx:latest
* redis:3.2
* redis:3
* postgres:9.5

Execute o comando abaixo para visualizar todas as imagens que se encontram localmente na sua estação, nesse momento:

```
# docker image list
```

# Criando imagens docker commit

Há duas formas de criar imagens customizadas: com _commit_ e com _Dockerfile_.

Criando imagens com _commit_

É possível criar imagens executando o comando commit, relacionado a um container. Esse comando usa o status atual do container escolhido e cria a imagem com base nele.

Vamos ao exemplo. Primeiro criamos um container qualquer:

```
# docker run -it --name meucontainer ubuntu:16.04 bash
```
Agora que estamos no bash do container, instalamos o nginx:

```
# apt-get update \
  apt-get install nginx -y \
  exit
```
Paramos o container com o comando abaixo:
```
# docker stop meucontainer
```
Agora, efetuamos o commit desse container em uma imagem:
```
# docker commit meucontainer meuubuntu:nginx
```
No exemplo do comando acima, _meucontainer_  é o nome do container criado e modificado nos passos anteriores; o nome _meuubuntu:nginx_ é a imagem resultante do commit; o estado do meucontainer  é armazenado em uma imagem chamada _meuubuntu:nginx_ que, nesse caso, a única modificação que temos da imagem oficial do ubuntu na versão 16.04 é o pacote nginx instalado.

Para testar a image criada basta executar:
```
# docker run -it --rm meuubuntu:nginx dpkg -l nginx
```

# Criando imagens Dockerfile

Quando se utiliza _Dockerfile_ para gerar uma imagem, basicamente, é apresentada uma lista de instruções que serão aplicadas em determinada imagem para que outra imagem seja gerada com base nas modificações.

Podemos resumir que o arquivo Dockerfile, na verdade, representa a exata diferença entre uma determinada imagem, que aqui chamamos de base, e a imagem que se deseja criar. Nesse modelo temos total rastreabilidade sobre o que será modificado na nova imagem.

Execute os passos abaixo para criar a sua imagem utilizando o _Dockerfile_.

Crie um arquivo chamado Dockerfile e dentro dele o seguinte conteúdo:

```
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install nginx
RUN apt-get install php5
COPY arquivo_teste /tmp/arquivo_teste
CMD bash
```
Crie um arquivo chamado arquivo_teste.

Vamos analisar o comando acima:

FROM para informar qual imagem usaremos como base, nesse caso foi ubuntu:16.04.

_RUN_ para informar quais comandos serão executados nesse ambiente para efetuar as mudanças necessárias na infraestrutura do sistema. São como comandos executados no shell do ambiente, igual ao modelo por commit, mas nesse caso foi efetuado automaticamente e, é completamente rastreável, já que esse Dockerfile será armazenado no sistema de controle de versão.

_COPY_ é usado para copiar arquivos da estação onde está executando a construção para dentro da imagem. Usamos um arquivo de teste apenas para exemplificar essa possibilidade, mas essa instrução é muito utilizada para enviar arquivos de configuração de ambiente e códigos para serem executados em serviços de aplicação.

_CMD_ para informar qual comando será executado por padrão, caso nenhum seja informado na inicialização de um container a partir dessa imagem. No exemplo, colocamos o comando bash, se essa imagem for usada para iniciar um container e não informamos o comando, ele executará o bash.

Com o seu arquivo salvo execute o comando:
```
docker image build -t meuubuntu:nginx_auto .
```
O comando tem a opção “-t”, serve para informar o nome da imagem a ser criada. No caso, será meuubuntu:nginx_auto e o “.” ao final, informa qual contexto deve ser usado nessa construção de imagem. Todos os arquivos da pasta atual serão enviados para o serviço do docker e apenas eles podem ser usados para manipulações do Dockerfile (exemplo do uso do COPY).

# Enviando a sua imagem para o docker hub

Agora que criamos a nossa propria imagem, então vamos subir para o docker hub. Para isso execute os passos abaixo:

Criar conta

Entre no site e crie uma conta. No cadastro você vai definir a sua Docker ID, que será seu identificador em cada repositório que criar. Assim, toda vez que você for usar a imagem será docker_id/nome_do_repositorio.

Criar repositório

Assim que fizer o login, você será direcionado para a página que lista seus repositórios criados. Basta clicar em Create Repository e seguir os passos.

Crie a tag

```
# docker tag -t meuubuntu:nginx_auto new-repo:tagname
```
```
# docker push new-repo/teste:tagname
```
_Obs:_ Talvez seja necessário você realizar a logar no docker hub usando o terminal para conseguir enviar a imagem. Para isso basta executar o comando:

```
# docker login
```

Pronto. Esta imagem já pode ser utilizada.

![Docker network](./imagens/welcomenginx.png)

