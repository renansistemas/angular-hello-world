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