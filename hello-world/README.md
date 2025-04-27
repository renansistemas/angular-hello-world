# angular-hello-world
app basico hello world usando angular

## Passos para criar e rodar o projeto Angular Hello World

### Pré-requisitos
Certifique-se de que você tenha as seguintes ferramentas instaladas:
- [Node.js](https://nodejs.org/) (versão LTS recomendada)
- [Angular CLI](https://angular.io/cli) (instalado globalmente)

### 1. Instalar o Angular CLI
Execute o seguinte comando no terminal para instalar o Angular CLI globalmente:
```bash
npm install -g @angular/cli
```

### 2. Criar um novo projeto Angular
Crie um novo projeto chamado `angular-hello-world` com o comando:
```bash
ng new angular-hello-world
```
Durante o processo, você será solicitado a escolher algumas opções:
- **Would you like to add Angular routing?** Escolha `No`.
- **Which stylesheet format would you like to use?** Escolha o formato desejado, como `CSS`.

### 3. Navegar para o diretório do projeto
Entre no diretório do projeto recém-criado:
```bash
cd angular-hello-world
```

### 4. Rodar o servidor de desenvolvimento
Inicie o servidor de desenvolvimento Angular com o comando:
```bash
ng serve
```

### 5. Acessar o aplicativo no navegador
Abra o navegador e acesse o endereço:
```
http://localhost:4200
```
Você verá a página inicial padrão do Angular, indicando que o aplicativo está funcionando.

### 6. Personalizar o aplicativo Hello World
Edite o arquivo `src/app/app.component.html` para exibir uma mensagem personalizada:
```html
<h1>Hello World com Angular!</h1>
```

Salve o arquivo e veja as alterações refletidas automaticamente no navegador.

### 7. Parar o servidor de desenvolvimento
Para parar o servidor, pressione `Ctrl + C` no terminal onde o comando `ng serve` está sendo executado.

### Recursos adicionais
- [Documentação oficial do Angular](https://angular.io/docs)
- [Guia de início rápido do Angular](https://angular.io/start)

Pronto! Agora você tem um aplicativo Angular Hello World funcionando.

### 8. Usando Docker e Nginx para rodar a aplicação

#### Criando o Dockerfile
Um `Dockerfile` foi criado para containerizar a aplicação Angular com Nginx. Ele contém as seguintes instruções:

```dockerfile
# Etapa 1: Construção da aplicação Angular
FROM node:22 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --prod

# Etapa 2: Configuração do Nginx
FROM nginx:alpine
COPY --from=build /app/dist/angular-hello-world /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Criando o arquivo de configuração do Nginx
Crie um arquivo chamado `nginx.conf` no mesmo diretório do `Dockerfile` com o seguinte conteúdo:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri /index.html;
    }

    error_page 404 /index.html;
}
```

#### Comandos para rodar o container

1. **Construir a imagem Docker**  
    Execute o comando abaixo no terminal para criar a imagem Docker da aplicação:
    ```bash
    docker build -t angular-hello-world-nginx .
    ```

2. **Rodar o container**  
    Após a imagem ser criada, execute o seguinte comando para iniciar o container:
    ```bash
    docker run -d -p 8080:80 --name angular-hello-world-container angular-hello-world-nginx
    ```

3. **Acessar a aplicação no navegador**  
    Abra o navegador e acesse o endereço:
    ```
    http://localhost:8080
    ```

    Você verá a aplicação Angular rodando dentro de um container Docker com Nginx.

4. **Parar e remover o container**  
    Para parar o container, execute:
    ```bash
    docker stop angular-hello-world-container
    ```
    Para remover o container, execute:
    ```bash
    docker rm angular-hello-world-container
    ```

### Observação
Certifique-se de que o Docker está instalado e em execução no seu sistema antes de executar os comandos acima.

### Recursos adicionais sobre Docker e Nginx
- [Documentação oficial do Docker](https://docs.docker.com/)
- [Guia de início rápido do Docker](https://docs.docker.com/get-started/)
- [Documentação oficial do Nginx](https://nginx.org/en/docs/)