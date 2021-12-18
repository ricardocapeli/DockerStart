# **Docker Compose**

O Docker Compose nada mais é do que uma forma de você conseguir escrever em um único arquivo todos os detalhes do ambiente de sua aplicação. Antes nós usávamos o dockerfile apenas para criar imagens, seja da minha aplicação, do meu BD ou do meu webserver, mas sempre de forma unitária, pois tenho um dockerfile para cada "tipo" de container: um para a minha app, outro para o meu BD e assim por diante.

Com o Docker Compose nós falamos sobre o ambiente inteiro. Por exemplo, no Docker Compose nós definimos quais os services que desejamos criar e quais as características de cada service (quantidade de containers debaixo daquele service, volumes, network, secrets, etc.).

O padrão que os compose files seguem é o YML, supersimples e de fácil entendimento, porém sempre é bom ficar ligado na sintaxe que o padrão YML lhe impõe. ;)


O arquivo abaixo ilustra como é um arquivo do Docker Compose e como declaramos os serviços:

```
version: "3"
services:
  mysql:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=wordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress

  wordpress:
    depends_on:
      - db 
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress 
 
volumes:
   db_data:

```
Em resumo, utilizando o Docker Compose, em vez de o administrador executar o docker run na mão para cada container e subir os serviços separados, linkando os containers das aplicações manualmente, temos um único arquivo que vai fazer essa orquestração e vai subir os serviços/containers de uma só vez. Isso diminui a responsabilidade do Sysadmin ou do desenvolvedor de ter que gerenciar o deploy e se preocupar em rodar todos esses comandos para ter a sua aplicação rodando com todas as suas dependências.

Vamos conhecer algumas opções que utilizamos anteriormente:

* _version: \"3\"_ Versão do compose que estamos utilizando.
* _services:_ Início da definição de meu serviço.
* _db:_ Nome do serviço.
* _image: mysql:5.7_ Imagem que vamos utilizar.
* _volumes_ Neste caso estamos mapeando um volume.
* _restart_ Caso o container ocorra algum erro ele irá reiniciar.
* _environment:_ Passamos as variaveis de ambiente para iniciar o serviço.
* _ports: 8000:80_ Quais portas desejamos expor.

Para subir este serviço basta executar o comando:
```
# docker-compose up -d
```

Pronto, através de um comando subimos dois serviços. Isso é bem diferente de executar docker run para cada um dos serviços.
