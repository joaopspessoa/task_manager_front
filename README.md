# Task Manager Front

Este é o front-end do projeto Task Manager, desenvolvido com Vue.js e Nuxt.js. Este projeto oferece uma interface de usuário para gerenciar tarefas de forma eficiente.

## Tecnologias Utilizadas

- [Vue.js](https://vuejs.org/): Framework JavaScript para construção de interfaces de usuário.
- [Nuxt.js](https://nuxtjs.org/): Framework baseado em Vue.js para aplicações universais.
- [TypeScript](https://www.typescriptlang.org/): Superconjunto tipado de JavaScript.
- [@vueuse/nuxt](https://vueuse.org/guide/nuxt.html): Plugin VueUse para Nuxt.js.
- [Docker](https://www.docker.com/): Plataforma para desenvolvimento e execução de aplicações em contêineres.
- [Docker Compose](https://docs.docker.com/compose/): Ferramenta para definir e executar aplicações Docker multi-contêiner.

## Dependências

### Dependências Principais

- **`nuxt`**: Framework para desenvolvimento de aplicações Vue.js.
- **`vue`**: Framework JavaScript para construção de interfaces de usuário.
- **`typescript`**: Superconjunto tipado de JavaScript.
- **`@vueuse/nuxt`**: Plugin VueUse para Nuxt.js.

### Dependências de Desenvolvimento

- **`@nuxt/typescript-build`**: Suporte TypeScript para Nuxt.js.

## Requisitos

- **Node.js**: Versão 18 ou superior.
- **Docker**: Plataforma para contêineres.
- **Docker Compose**: Ferramenta para compor contêineres Docker.

## Configuração do Projeto

### Passo a Passo

1. **Clone o repositório:**

   ```bash
   git clone https://github.com/joaopspessoa/task_manager_front.git
   cd task-manager-front
   ```

2. **Construir e Iniciar o Contêiner:**

   Construa e inicie o contêiner Docker com o seguinte comando:

   ```bash
   docker-compose up -d --build
   ```

4. **Acessar a Aplicação:**

   Após iniciar o contêiner, você pode acessar a aplicação no navegador através do seguinte URL:

   ```bash
   http://localhost:3000
   ```
