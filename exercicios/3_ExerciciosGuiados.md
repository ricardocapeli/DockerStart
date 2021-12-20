# Exercicios guiados

### 1 - Provisione um wordpress e o configure persistindo os dados.
#### 1.1 Crie uma pasta, nos vamos utilizá-la para todos arquivos necessário para o provisionamento dos serviços e dados. Deposi de criada crie um arquivo chamado _docker-compose.yml_. 

#### 1.2 Neste arquivo devemos inserir os seguintes itens:

* O arquivo deve ser iniciado com a _versão_ do docker-compose. Na maioria dos casos devemos utilizar sempre a ultima versão disponível, mas sempre respeite o projeto em que este atuando. Para saber qual a ultima versão basta acessar o [portal com as versões](https://docs.docker.com/compose/compose-file/). 
  
```
version: "3.8" 
```
* Em seguida, defina a lista de serviços (ou container) que você deseja executar como parte do seu aplicativo.
  
```
services:
```  
* Abaixo da instrução _services_, você irá adicionar os serviços que serão executados. Você que irá definir o nome do seu serviço. Para o nosso exemplo vamos iniciar com os dados do banco de dados que iremos utilizar. 

```
db:
```

* Agora devemos inserir a imagem que vamos utilizar do banco de dados.

```
image: mysql:5.7
```
* Crie uma pasta onde este arquivo docker-compose.yml com o nome _mysql_. Com a pasta criada vamos fazer o mapeamento da pasta para o nosso container. Dessa forma os dados do banco de dados estarão salvos e caos precise baixar e subir novamente ele subirá com as informações armazenadas.

```
volumes:
   - ./mysql/db_data:/var/lib/mysql
```

* Adicione a opção de sempre reiniciar o container caso ocorra algum erro que venha finalizar o container.

```
restart: always
```
* Adicione as variáveis do mysql para que ao iniciar o container o mysql já seja configurado.

```
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
```

* Vamos agora passar as informações do nosso segundo serviço, o wordpress. Adicionar o nome do nosso serviço.

```
wordpress:
```

* Para este exemplo nós precisamos que o wordpress só seja iniciado após o mysql estar pronto. Para isto vamos adicionar o _depends_on_. Nele você pode indicar o nome do serviço que você quer que anteceda ao seu. 

```
     depends_on:
       - db
```

* Vamos adicionar a imagem que desejamos para o wordpress.

```
image: wordpress:latest
```

* Para que seja possível acessar a pagina do wordpress em nossa máquinas, vamos adicionar o redirecionamento de portas.

```
     ports:
       - "8000:80"
```

* Vamos adicionar a opção de restart caso o container seja encerrado inesperadamente.

```
restart: always
```
* Da mesma forma que foi feito para o serviço do mysql, precisamos adicionar as variáveis de ambiente para que o nosso wordpress se conecte ao banco de dados.

```
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
```

* O seu docker-compose.yml deve ficar da seguinte maneira:

```
version: '3.8'

services:
   db:
     image: mysql:5.7
     volumes:
       - ./mysql/db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     volumes:
      - ./wordpress/wordpress:/var/www/html     
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
```

_Obs:_ O Docker Compose nos possibilita uma várias possibilidade para provisionar os nossos serviços, portanto adicionamos apenas algumas configurações para os nossos serviços, você pode procurar mais na documentação para facilitar o provisionamento dos serviços.

#### 1.3 Com as configurações devidamente pré-definidas vamos iniciar os nossos serviços.

```
# docker-compose up -d
```
Apos o termino dos procedimento que o docker irá realizar como o download das imagens basta acessar e configurar o wordpress acessando o endereço https://locahost:8000
