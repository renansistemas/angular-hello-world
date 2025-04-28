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

## Usando Docker Compose para Gerenciar o Ambiente

### O que estamos fazendo no `docker-compose.yml`

O arquivo `docker-compose.yml` é usado para definir e gerenciar serviços Docker de forma simplificada. Ele permite que você configure múltiplos containers e suas interações em um único arquivo YAML. No caso do projeto Angular Hello World, utilizamos o `docker-compose.yml` para orquestrar o container da aplicação Angular com Nginx.

### Estrutura do `docker-compose.yml`

Aqui está o conteúdo do arquivo `docker-compose.yml`:

```yaml
services:
    backend:
        image: renansis/hello-world-java:1.0
        ports:
            - "8080:8080"
        networks:
            - app-network

    frontend:
        build:
            context: .
            dockerfile: Dockerfile
        ports:
            - "4200:80"
        networks:
            - app-network

networks:
    app-network:
        driver: bridge
```

### Explicação dos elementos:

1. **services**: Define os serviços que serão gerenciados. Neste caso, temos dois serviços:
     - **backend**: Um container baseado na imagem `renansis/hello-world-java:1.0`, que expõe a porta `8080`.
     - **frontend**: Um container que será construído a partir do `Dockerfile` no diretório atual (`./`) e expõe a porta `4200`.

2. **networks**: Define uma rede chamada `app-network` com o driver `bridge`, permitindo que os containers se comuniquem entre si.

### Passos para usar o `docker-compose.yml`

1. **Criar o arquivo `docker-compose.yml`**  
     No diretório raiz do projeto, crie um arquivo chamado `docker-compose.yml` e copie o conteúdo acima.

2. **Construir e iniciar os serviços**  
     Execute o seguinte comando para construir as imagens e iniciar os containers:
     ```bash
     docker-compose up --build
     ```

3. **Acessar os serviços no navegador**  
     - Para o backend, acesse:
         ```
         http://localhost:8080
         ```
     - Para o frontend, acesse:
         ```
         http://localhost:4200
         ```

4. **Parar os serviços**  
     Para parar os serviços, pressione `Ctrl + C` no terminal onde o comando `docker-compose up` está sendo executado.

5. **Remover os containers e redes criados**  
     Execute o seguinte comando para limpar os recursos criados pelo Docker Compose:
     ```bash
     docker-compose down
     ```

### Benefícios do Docker Compose

- **Gerenciamento simplificado**: Permite gerenciar múltiplos serviços em um único arquivo.
- **Reprodutibilidade**: Garante que o ambiente seja consistente em diferentes máquinas.
- **Facilidade de configuração**: Reduz a necessidade de executar múltiplos comandos manualmente.

Com este `docker-compose.yml`, você pode gerenciar facilmente o backend e o frontend do projeto Angular Hello World em um ambiente Docker integrado.

#### Explicação dos elementos:

1. **version**: Define a versão do Docker Compose. Aqui usamos a versão `3.8`, que é compatível com a maioria dos recursos modernos do Docker.

2. **services**: Define os serviços que serão gerenciados. No nosso caso, temos um único serviço chamado `angular-app`.

3. **build**: Especifica como construir a imagem Docker:
     - **context**: Define o diretório de contexto para a construção da imagem. Aqui usamos o diretório atual (`.`).
     - **dockerfile**: Especifica o nome do arquivo Dockerfile a ser usado.

4. **ports**: Mapeia as portas do container para o host. Aqui, a porta `80` do container é mapeada para a porta `8080` do host.

5. **volumes**: Monta um volume para sincronizar os arquivos do host com o container. Isso permite que as alterações feitas no diretório `dist/angular-hello-world` sejam refletidas no container.

6. **container_name**: Define um nome personalizado para o container, facilitando sua identificação.

### Passos para usar o `docker-compose.yml`

1. **Criar o arquivo `docker-compose.yml`**  
     No diretório raiz do projeto, crie um arquivo chamado `docker-compose.yml` e copie o conteúdo acima.

2. **Construir e iniciar os serviços**  
     Execute o seguinte comando para construir a imagem e iniciar o container:
     ```bash
     docker-compose up --build
     ```

3. **Acessar a aplicação no navegador**  
     Após o container ser iniciado, abra o navegador e acesse:
     ```
     http://localhost:8080
     ```

4. **Parar os serviços**  
     Para parar os serviços, pressione `Ctrl + C` no terminal onde o comando `docker-compose up` está sendo executado.

5. **Remover os containers e redes criados**  
     Execute o seguinte comando para limpar os recursos criados pelo Docker Compose:
     ```bash
     docker-compose down
     ```

### Benefícios do Docker Compose

- **Automação**: Simplifica o processo de construção e execução de containers.
- **Reprodutibilidade**: Garante que o ambiente seja consistente em diferentes máquinas.
- **Facilidade de uso**: Reduz a necessidade de executar múltiplos comandos Docker manualmente.

Com o `docker-compose.yml`, gerenciar o ambiente Docker do projeto Angular Hello World se torna mais eficiente e organizado.
