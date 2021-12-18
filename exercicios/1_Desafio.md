# Exercicios

### 1 Execute um container baseado nas informações abaixo:
* Baixe a imagem quay.io/redhattraining/httpd-parent;
* Use a tag 2.4 da imagem;
* O nome do container deve chamar httpd-basic;
* O container deve ser executado em background;
* O serviço deve ser disponivel na porta 8080 e redirecionar para a porta 80 do container.

Para ter certeza que o container esta executando verifique atraves de linha de comando e depois atraves do seu navegador utilizando o endereço http://localhost:8080. Deve aparecer a mensage "Hello from the httpd-parent container!"

**Obs**
Conforme vimos, o docker hub não único repositório de imagens existem outros, alguns publicos e outros privados. No seu dia a dia você encontrará as informações necessárias para utilizar a imagem correta. Neste exemplo estamos utilizando o quay.io para baixar a imagem httpd-parent utilizando a tag 2.4.
</br>

### 2 Customize a mensagem apresentada em seu navegador quando é acessado o endereço http://localhost:8080.

* Acesse o seu container que esta sendo executado;
* Dentro do container acessar o endereço /var/www/html e verifique se dentro desta pasta existe um arquivo index.html;
* Ainda dentro do container, subistitua a mensagem atual por uma nova "Ola mundo!" que esta no arquivo index.html.
* Saia do container e verifique se a nova mensagem foi inserida. 
</br>

### 3 Pare o container que esta sendo executado.
