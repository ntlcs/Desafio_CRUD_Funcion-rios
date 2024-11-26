# Sistema de Gestão de Funcionários com FastAPI

Este projeto implementa um sistema básico de gestão de funcionários usando **FastAPI**, **SQLite** e **JWT** para autenticação. O sistema permite a criação, visualização, atualização e exclusão de perfis de funcionários. Ele também implementa uma funcionalidade de login com autenticação baseada em token.

## Funcionalidades

- **Autenticação**: Um sistema de login que utiliza JWT (JSON Web Token) para autenticar os usuários e proteger os endpoints da API.
- **Gestão de Funcionários**: Permite aos administradores (superusuários e gestores) criar, visualizar, atualizar e excluir perfis de funcionários.
- **Segurança**: A senha dos usuários é criptografada usando o algoritmo **bcrypt**, garantindo a segurança dos dados de acesso.

## Tecnologias Utilizadas

- **FastAPI**: Framework web para construção de APIs rápidas e eficientes.
- **SQLite**: Banco de dados relacional utilizado para armazenar os dados de funcionários e usuários.
- **JWT**: Tecnologia para autenticação e autorização de usuários com tokens seguros.
- **PassLib**: Biblioteca para criptografia de senhas.
- **Pyngrok**: Ferramenta para criar um túnel e expor a aplicação local à internet durante o desenvolvimento.
- **Uvicorn**: Servidor ASGI para rodar a aplicação FastAPI.

## Estrutura do Projeto

### 1. **Banco de Dados (SQLite)**
O banco de dados contém duas tabelas principais:

- **usuarios**: Armazena informações dos usuários, incluindo email, senha criptografada e perfil (superusuário ou gestor).
- **funcionarios**: Contém dados dos funcionários, como nome, sobrenome, usuário, departamento e email.

### 2. **EndPoints**

- **POST /login**: Realiza o login do usuário e retorna um token JWT válido.
- **POST /funcionarios**: Permite a criação de novos perfis de funcionários. Requer um token de autenticação.
- **GET /funcionarios**: Exibe a lista de funcionários. Filtra por departamento se o usuário for um gestor.
- **PUT /funcionarios/{funcionario_id}**: Atualiza as informações de um funcionário. Requer permissão de um superusuário ou gestor.
- **DELETE /funcionarios/{funcionario_id}**: Deleta o perfil de um funcionário. Requer permissão de um superusuário ou gestor.

### 3. **Segurança**
- **JWT (JSON Web Token)**: Utilizado para autenticar usuários e proteger endpoints da API. O token é gerado após o login e deve ser incluído nos cabeçalhos das requisições aos endpoints protegidos.
- **Criptografia de Senhas**: As senhas dos usuários são criptografadas utilizando o algoritmo **bcrypt** para garantir a segurança no armazenamento.

## Como Executar o Projeto

### Requisitos

- Python 3.10 ou superior
- Bibliotecas Python: FastAPI, Uvicorn, Pyngrok, PassLib, SQLite, JWT.

### Passos para Rodar a Aplicação

1. **Instalar as dependências**:

```bash
pip install fastapi uvicorn pyngrok passlib sqlite3 jwt
```

2. **Criar o banco de dados e as tabelas**:

   O código cria automaticamente as tabelas `usuarios` e `funcionarios` ao ser executado pela primeira vez. Além disso, ele insere um usuário de exemplo com a senha criptografada.

3. **Iniciar o servidor**:

   Para rodar o servidor, basta executar o comando:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

   O servidor estará disponível em `http://localhost:8000`.

4. **Expor a aplicação usando o Ngrok**:

   A aplicação pode ser exposta para a internet usando o Ngrok para testes externos. Após rodar o servidor localmente, basta iniciar o túnel:

```bash
ngrok http 8000
```

   Isso gerará um link público (ex.: `http://abc123.ngrok.io`) que pode ser utilizado para acessar a API.

## Exemplo de Uso

### 1. **Login do Usuário**

   Envie uma requisição POST para `/login` com o seguinte corpo JSON:

```json
{
  "email": "natalia@exemplo.com",
  "senha": "NATALIA",
  "perfil": "super"
}
```

   Se as credenciais estiverem corretas, você receberá um token JWT:

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### 2. **Criar Funcionário**

   Com o token JWT, faça uma requisição POST para `/funcionarios` com o seguinte corpo:

```json
{
  "nome": "João",
  "sobrenome": "Silva",
  "usuario": "joao.silva",
  "departamento": "TI",
  "email": "joao.silva@empresa.com"
}
```

   Se o token for válido e você tiver permissão, o funcionário será criado com sucesso.

### 3. **Visualizar Funcionários**

   Para visualizar os funcionários, faça uma requisição GET para `/funcionarios`, passando o token de autenticação no cabeçalho:

```bash
Authorization: Bearer <seu_token_jwt>
```

## Conclusão

Este projeto é uma implementação básica de um sistema de gestão de funcionários com autenticação e autorização via JWT. Ele pode ser expandido para incluir mais funcionalidades, como gestão de projetos, relatórios, entre outros.
