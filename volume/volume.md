# Armazenamento Permanentes

Quando iniciamos um container a partir de uma imagem, a imagem pode ser considerada um pacote “fechado” de uma aplicação, ou seja, a aplicação que está sendo executada em um container faz o processamento mas tem por padrão salvar dados dentro do container.

Imagine você subir um mysql em um container e a sua aplicação se conectou ao banco, e armazenar várias informações de um portal de cadastro de clientes, caso ocorra que o container que está rodando a imagem do mysql venha ser finalizado, por qualquer motivo, todos os seu dados serão perdidos.

É neste momento que precisamos ter volumes que persistam os dados, ou seja, que caso o container venha ser finalizado os dados sejam salvos.

Os volumes nada mais são que diretórios externos ao container que está sendo executado, é como um HD externo, que quando você iniciar um mysql, você pode apresentar uma pasta que pode estar na sua máquina ou até mesmo compartilhada na rede e então os dados ficarão salvos naquele local.

Um contêiner em execução obtém uma nova camada sobre sua imagem de contêiner de base, e essa camada é o armazenamento do contêiner. A princípio, essa camada é o único armazenamento de leitura / gravação disponível para o contêiner e é usada para criar arquivos de trabalho, arquivos temporários e arquivos de log. Esses arquivos são considerados voláteis. Um aplicativo não para de funcionar se forem perdidos. A camada de armazenamento do contêiner é exclusiva do contêiner em execução, portanto, se outro contêiner for criado a partir da mesma imagem de base, ele obterá outra camada de leitura / gravação. Isso garante que os recursos de cada contêiner sejam isolados de outros contêineres semelhantes.

## Vamos criar um container e uma pasta em nosso desktop e persitir os dados.

Criei uma pasta em algum diretório em seu desktop

```
# mkdir ~/Desktop/Work/study/docker/vol
```
Dentro desta basta crie um arquivo chamado index.html. Dentro do arquivo copie o html abaixo e coloque o seu nome.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Hello World - Nginx Docker</title>
    <style>
        h1{
            font-weight:lighter;
            font-family: Arial, Helvetica, sans-serif;
        }
    </style>
</head>
<body>
    
    <h1>
        Ricardo
    </h1>

</body>
</html>
```

Apos isto inicie o seu container com o seguinte comando:
```
docker run -d -p 8080:80 -v $(pwd)/vol:/usr/share/nginx/html nginx
```
Neste comando foram utilizados alguns parâmetros:
* _-p_ mapeamento de portas.
* _-v_ – mapeamento do volume
* _$(pwd)/vol_ a pasta que criamos que vai ser utilizada para persitir os dados.
* _: (dois pontos)_ faz a separação do volume do seu host e onde vai ser mapeado dentro do container.
* _/usr/share/nginx/html_ neste caso é onde fica as configurações de Hello World do serviço nginx.

Acesse o endereço atraves do endereço http://locahost:8080 e veja o seu nome.