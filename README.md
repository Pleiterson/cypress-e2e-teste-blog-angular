<!-- Badges session -->
<p align="center">
  <!-- languages -->
  <img src="https://img.shields.io/github/languages/count/pleiterson/api-previsao-tempo-angular?style=social" alt="Linguagens utilizadas">
  <!-- repo size -->
  <img src="https://img.shields.io/github/repo-size/Pleiterson/api-previsao-tempo-angular?style=social" alt="Tamanho do repositório">
  <!-- last commit -->
  <img src="https://img.shields.io/github/last-commit/Pleiterson/api-previsao-tempo-angular?style=social" alt="Último commit">
  <!-- licence MIT -->
  <img src="https://img.shields.io/github/license/Pleiterson/api-previsao-tempo-angular?style=social" alt="Licença MIT">
</p>

<!--Banner session-->
<p align="center">
  <img src="./assets/readme/banner.png" alt="DIO" title="Digital Innovation One">
</p>

<!--About session-->
<h1 align="center">Utilizando o Cypress E2E para testar um Blog em Angular<br>Digital Innovation One</h1>

<img src="./assets/readme/badge.png" title="Badge" width="70" height="70">

Curso do Bootcamp React Native Mobile Developer da [Digital Innovation One](https://digitalinnovation.one/).

Neste projeto você terá o desafio de construir um teste E2E com Cypress para um Blog em Angular.

<h3>Testes realizados</h3>

- Primeiro rode os comando abaixo para baixar um projeto para a execução dos testes:

```
git clone https://github.com/gothinkster/angular-realworld-example-app

cd angular-realworld-example-app

npm install

npm run start
```
- Depois no navegador de internet digite: http://localhost:4200/
<br>
- Adicionando o Cypress...
- Abra outro terminal. Na pasta/angular-realworld-example-app/ execute:
```
npm install cypress --save-dev

npx cypress -v
```
- Caso tenha problemas com Proxy ou Firewall, baixe o [binário](https://download.cypress.io/desktop) e configure a variável de ambiente antes de instalar:

```
set CYPRESS_INSTALL_BINARY=C:\cypress.zip

npm install cypress --save-dev --verbose
```
- Remova o pacote:
```
npm uninstall protactor --save-dev
```
- Exclua a pasta /e2e/
- Do package.json, remova a linha: "e2e": "ng e2e"
- O comando "cypress open", além de abrir o Cypress Test Runner, cria a pasta inicial cypress e o arquivo de configuração /cypress.json
- E já vem com /examples/ dos principais comandos Cypress.
```
npx cypress open
```
<br>
- Primeiro teste...
- Crie um arquivo no caminho "cypress/integrations/examples" com o nome "exemplo.spec.js", e dentro dele adicione o código abaixo

```
describe('Primeiro Teste', () => {
  it('Exemplos Cypress', () => {
    cy.visit('https://example.cypress.io')
    expect(true).to.equal(true)
  })
})
```
describe and it come from Mocha<br>
expect comes from Chai

- Rodando os testes
- Para executar todos os testes da pasta /cypress/integration/:
```
npx cypress run
```
- Para executar somente um roteiro:
```
npx cypress run --spec "cypress/integration/examples/exemplo.spec.js"
```
- Para abrir a interface do Cypress Test Runner:
```
npx cypress open
```
<br>
- Configuração JSON
- Abra o arquivo cypress.json e adicione o código abaixo

```
{
  "baseUrl": "http://localhost:4200",
  "pageLoadTimeout": 30000,
  "defaultCommandTimeOut": 30000,
  "viewportHeight": 800,
  "viewportWidth": 500,
  "retries": 3
}
```
<br>

- Roteiro dos testes...
1. Cadastro
2. Login
3. Perfil
4. Feeds
5. Paginação
6. Post
7. Tags
8. Comentários
9. Seguir
10. Logout
<br>
- Cypress Recorder
- [Extensão](https://chrome.google.com/webstore/detail/cypress-recorder/glcapdcacdfkokcmicllhcjigeodacab) para o Chrome capaz de gravar um roteiro base. Recomendado para capturar os seletores no DOM.
<br>
1. Cadastro: crie o arquivo "cadastro.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Conduit Cadastro', () => {
    const usuario = 'usuario' + (new Date()).getTime()
    const senha = 'senha' + (new Date()).getTime()

    it('Novo Usuário', () => {
        cy.visit('/register')
        cy.get('[formcontrolname=username]').type(usuario)
        cy.get('[formcontrolname=email]').type(usuario+'@email.com')
        cy.get('[formcontrolname=password]').type(senha)
        cy.get('.btn').click()
        cy.contains('.nav-item:nth-child(4) > .nav-link', usuario)
            .should('be.visible')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/cadastro.spec.js
```
2. Login
- Support Comands: abra o arquivo "index.js" no caminho "cypress/support" e adicione o código abaixo no final do que já existe.
```
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/login')
  cy.url().should('include', '/login')
  cy.get('[formcontrolname=email]').type(username)
  cy.get('[formcontrolname=password]').type(password)
  cy.get('.btn').click()
})
```
- Crie o arquivo "login.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Conduit Login', () => {
    it('Login sucesso', () => {
        cy.login('testecypress@testecypress.com', 'testecypress')
        cy.get('.nav-item:nth-child(4) > nav-link').click()
        cy.get('.btn:nth-child(5)').click()
        cy.url().should('contain', '/settings')
    })

    it('Dados Inválidos', () => {
        cy.login('usuario@inexistente.com', 'senhaerrada')
        cy.get('.error-messages > li')
            .should('contain', 'email or password is invalid')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/login.spec.js
```
3. Perfil: crie o arquivo "perfil.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Profile', () => {
    it('Editar Perfil', () => {
        cy.login('testecypress@testecypress.com', 'testecypress')
        cy.contains('testecypress').click()
        cy.contains('Edit Profile Settings').click()
        cy.get('[formcontrolname="image"]').clear()
        cy.get('[formcontrolname="image"]')
            .type('https://thispersondoesnotexist.com/image')
        cy.get('[formcontrolname="password"]').type('testecypress')
        cy.contains('Update Settings').click()
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/perfil.spec.js
```
4. Feeds: crie o arquivo "feeds.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Conduit Feed', () => {
    it('Ver Feeds', () => {
        cy.login('testecypress@testecypress.com', 'testecypress')
        cy.get('.nav-pills > .nav-item:nth-child(1) > .nav-link').click();
        cy.get('.nav-pills > .nav-item:nth-child(2) > .nav-link').click()
        cy.get('app-article-preview:nth-child(1) .btn').click()
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/feeds.spec.js
```
5. Paginação: crie o arquivo "pagination.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Paginação', () => {
    it('Paginar', () => {
        cy.visit('/')
        cy.get('.page-item.active > a').contains('1')
        cy.get('.page-item:nth-child(2) > .page-link').click()
        cy.get('.page-item.active > a').contains('2')
        cy.get('.page-item:nth-child(3) > .page-link').click()
        cy.get('.page-item.active > a').contains('3')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/pagination.spec.js
```
6. Post: crie o arquivo "post.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Post', () => {
    beforeEach(() => {
        cy.login('testecypress@testecypress.com', 'testecypress')
    })

    it('Novo', () => {
        const tit = 'Cypress E2E'
        cy.contains('New Article').click()
        cy.location('pathname').should('equal', '/editor')
        cy.get('[formcontrolname=title]').type(tit)
        cy.get('[formcontrolname=description]').type('Ponta a Ponta')
        cy.get('[formcontrolname=body]').type('Agilidade, Qualidade')
        cy.contains('Publish Article').click()
        cy.get('h1').contains(tit)
    })

    it('Editar', () => {
        cy.contains('testecypress').click()
        cy.location('pathname').should('contains', '/profile')
        cy.get('.article-preview').get('h1').first().click()
        cy.contains('Edit Article').click()
        cy.get('[formcontrolname=body]').clear()
        cy.get('[formcontrolname=body]').type('Economia')
        cy.contains('Publish Article').click()
        cy.contains('Economia')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/post.spec.js
```
7. Tags: crie o arquivo "tags.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Paginação', () => {
    it('Paginar', () => {
        cy.visit('/')
        cy.get('.page-item.active > a').contains('1')
        cy.get('.page-item:nth-child(2) > .page-link').click()
        cy.get('.page-item.active > a').contains('2')
        cy.get('.page-item:nth-child(3) > .page-link').click()
        cy.get('.page-item.active > a').contains('3')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/tags.spec.js
```
8. Comentários: crie o arquivo "comentarios.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Comentarios', () => {
    it('Escrever', () => {
        cy.login('testecypress@testecypress.com', 'testecypress')
        cy.contains('Global Feed').click()
        cy.get('.preview-link').first().click()
        cy.get('.form-control').type('Sensacional!')
        cy.get('.btn-primary').click()
        cy.contains('Sensacional!')
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/comentarios.spec.js
```
9. Seguir: crie o arquivo "seguir.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Seguir', () => {
    it('Seguir Usuário', () => {
        const usuario = 'usuario'+(new Date()).getTime();
        const senha = 'senha'+(new Date()).getTime();
        cy.visit('/register', { timeout: 10000 })
        cy.get('[formcontrolname=username]').type(usuario)
        cy.get('[formcontrolname=email]').type(usuario+'@email.com')
        cy.get('[formcontrolname=password]').type(senha)
        cy.get('.btn').click()
        cy.wait(10000)
        cy.visit('/profile/testecypress')
        cy.contains('Folow').click()
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/seguir.spec.js
```
10. Logout: crie o arquivo "logout.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
describe('Logout', () => {
    it('Logout via Perfil', () => {
        cy.login('testecypress@testecypress.com', 'testecypress')
        cy.contains('Settings').click()
        cy.url().should('include', '/settings')
        cy.get('.btn-outline-danger').click()
    })
})
```
- Execute o comando abaixo para executar este arquivo criado
```
npx cypress run --spec cypress/integration/logout.spec.js
```
<br>

- Fixture: Data-Driven Tests
- Crie o arquivo "index.spec.js" no caminho "cypress/support" e adicione o código abaixo
```
Cypress.Commands.add('loadUsers', () => {
  cy.fixture('users')
    .as('users')
})
```
- Crie o arquivo "test.spec.js" no caminho "cypress/integration" e adicione o código abaixo
```
// this.users.default.username
// this.users.default.pass
// this.users.client.username
// this.users.client.pass
```
- Crie o arquivo "users.spec.js" no caminho "cypress/fixtures" e adicione o código abaixo
```
{
  "default":{
    "user": "basic-user",
    "pass": "testpass"
  },
  "client":{
    "user": "premium-user",
    "pass": "testpass"
  }
}
```
- [Plugins](https://docs.cypress.io/plugins/) e Funcionalidades extendidas.
- DevOps
```
# cypress/Dockerfile

# Imagem base
FROM cypress/base:14.0.0

# Copia os arquivos para a imagem
COPY ./

# Opcional - Pega o binário local
#ENV CYPRESS_INSTALL_BINARY=/cypress.zip

RUN npm install --verbose
RUN $(npm bin)/cypress verify

# Inicialização do container
CMD $(npm bin)/cypress run
```
```
# prompt / terminal

docker build -t cypress:0.0.1
docker run cypress:0.0.1
```
<br>

- Paralelismo
```
cypress run --parallel --record
```
- Cypress Dashboard
```
cypress run --record --key=abc123
```
<br>

- Outros exemplos:
1 - Cypress Kitchen Sink
```
git clone
https://github.com/cypress-io/cypress-example-kitchensink.git

cd cypress-example-kitchensink
npm install
npm start
npm run cy:open
```
2 - Cypress Real Word App
```
git clone
https://github.com/cypress-io/cypress-realworld-app.git

cd cypress-realworld-app
npm install
npm dev
npm run cypress:open
```
<br>

- [Cheatsheet Mocha](https://devhints.io/mocha)
- [Cheatsheet Chai](https://devhints.io/chai)
- [Cheatsheet Cypress](https://cheatography.com/aiqbal/cheat-sheets/cypress-io/)
<!--License session-->
<h3>📝 Licença</h3>

- Este projeto está sob a licença [MIT](./LICENSE).

<!--Bottom session-->
<br><h4 align=center>Made with by <a target="_blank" href="https://pleiterson.vercel.app" >Pleiterson Amorim</a></h4>
