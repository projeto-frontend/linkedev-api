<h1 align="center">
  Projeto FrontEnd - API
</h1>

<p align = "center">
Este é a demonstração dos endpoints da aplicação do Projeto FrontEnd do M3 da Kenzie Academy, utilizando JSON Server.
</p>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

O url base da API é https://linkedev.herokuapp.com

### Changelog - 01/09

- User Dev não tem mais o parâmetro "role", a verificação fica a cargo do parâmetro booleano "is_recruiter" no user Recruiter;
- Parâmetro mudou de "recruiter" para "is_recruiter" para melhor semântica;
- Todos os parâmetros em português foram passados para inglês;
- Adicionado o endpoint "/jobs" para cadastrar vagas;
- Removidos parâmetros de id (é dado pela api automaticamente);
- Nível de permissão do endpoint "/users" alterado para 640;
- Adicionado o parâmetro "title" no user Dev;
- Adicionado o parâmetro "type" nas vagas para definir a informação remoto vs presencial;

<h2 align ='center'> Criação de Desenvolvedor</h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
[
  {
    "email": "linkedev@email.com",
    "password": "123456",
    "name": "Linkedev",
    "level": "Sênior",
    "title": "Desenvolvedor Frontend",
    "stacks": ["HTML", "CSS", "JavaScript"],
    "bio":  "Lorem ipsum dolor emet",
    "social": "github/johndoe",
    "avatar_url": "https://avatar.png",
  }
]
```

Caso dê tudo certo, a resposta será assim:

`POST /devs - FORMATO DA RESPOSTA - STATUS 201`

```json
[
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInRwiZXhwIjoxNjYxODk5MzA5LCJzdWIiOiIyIn0.WsEiwCoB1knJ3QgP9Yi97zl4bTzxHOcmspz7X1EARr8",
	"user": {
		"email": "linkedev@email.com",
		"name": "Linkedev",
		"level": "Sênior",
		"title": "Desenvolvedor Frontend",
		"stacks": [
			"HTML",
			"CSS",
			"JavaScript"
		],
		"bio": "Lorem ipsum dolor emet",
		"social": "github/johndoe",
		"avatar_url": "https://avatar.png",
		"role": "developer",
		"id": 2
	}
}
]
```

1. O campo - "social": Pode receber as redes sociais da pessoa, ou numero de telefone, algum método de contato fora da aplicação.

2. O campo - "stacks" é uma array de strings que guarda as tecnologias que o usuário domina.

<h2 align ='center'> Possíveis erros </h2>

Email já cadastrado:

`POST /devs`
` FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": "Email already exists"
}
```

<h2 align = "center"> Login </h2>

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "linkedev@email.com",
  "password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
[
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInRwiZXhwIjoxNjYxODk5MzA5LCJzdWIiOiIyIn0.WsEiwCoB1knJ3QgP9Yi97zl4bTzxHOcmspz7X1EARr8",
	"user": {
		"email": "linkedev@email.com",
		"name": "Linkedev",
		"level": "Sênior",
		"title": "Desenvolvedor Frontend",
		"stacks": [
			"HTML",
			"CSS",
			"JavaScript"
		],
		"bio": "Lorem ipsum dolor emet",
		"social": "github/johndoe",
		"avatar_url": "https://avatar.png",
		"id": 2
	}
}
]
```

Com essa resposta, vemos que temos duas informações, o dev e o token respectivo, dessa forma você pode guardar o token e o usuário logado no localStorage para fazer a gestão do dev no seu frontend.

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

Após o dev estar logado, para buscar o perfil do próprio dev, utilize o endpoint a seguir.

<h2 align ='center'> Buscar Perfil do usuário logado (token) </h2>

`GET /profile - FORMATO DA REQUISIÇÃO`

<blockquote>Na requisição apenas é necessário o TOKEN.</blockquote>

<br>

`GET /users - FORMATO DA RESPOSTA - STATUS 200`

```json
[
{
	"email": "linkedev@email.com",
	"password": "$2a$10$MbEJ6eUT2sHUrcEAQFvtN.rWz3QqlLmNm9lceIg0RemIvfv3Z2cb2",
	"name": "Linkedev",
	"level": "Sênior",
	"title": "Desenvolvedor Frontend",
	"stacks": [
		"HTML",
		"CSS",
		"JavaScript"
	],
	"bio": "Lorem ipsum dolor emet",
	"social": "github/johndoe",
	"avatar_url": "https://avatar.png",
	"id": 2
}
]
```

<h2 align ='center'> Atualizando os dados do perfil </h2>

Nesse é preciso estar logado, com o token no cabeçalho da requisição. Estes endpoints são para atualizar seus dados como, foto de perfil, nome, ou qualquer outra informação em relação ao que foi utilizado na criação do usuário.

Endpoint para atualizar a foto de perfil:

`PATCH /users/ - FORMATO DA REQUISIÇÃO`

```multipart
avatar: <Arquivo de imagem>
```


<h2 align ='center'> Criação de recrutador </h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
[
  {
    "name": "John Doe",
    "email": "johndoe@email.com",
    "password": "12345678",
    "company": "Google",
    "social": "github/johndoe",
    "avatar_url": "https://avatar.png",
    "is_recruiter": true
  },
]
```

<h2 align ='center'> Atualizando perfil de recrutador </h2>

`PUT /users - FORMATO DA REQUISIÇÃO`

```json
{
  "name": "John Doe",
  "contact": "linkedin/araujooj",
  "old_password": "123456",
  "password": "123456789"
}
```

<h2 align ='center'> Criando vagas </h2>

Aqui será preciso estar logado e possuir o cargo de Tech Recruiter;
<p>A data pode ser passada utilizando:</p>

```javascript
const today = new Date()
today.toLocaleDateString()
```

`POST /jobs`

```json
{
  "title": "Desenvolvedor Front-End",
  "description": "asas asasa...",
  "place": "São Paulo",
  "salary": 1000000,
  "level": "junior",
  "stacks": ["html", "css", "javascript"],
  "type": "Remoto",
  "reputation": 0,
  "candidates": [],
  "userId": "recruiter_id",
  "date": "30/08/2022"
}
```

`GET /jobs`
```json
{
  "title": "Desenvolvedor Front-End",
  "description": "asas asasa...",
  "place": "São Paulo",
  "salary": 1000000,
  "level": "junior",
  "stacks": ["html", "css", "javascript"],
  "type": "Remoto",
  "reputation": 0,
  "candidates": [],
  "userId": "recruiter_id",
  "date": "30/08/2022"
}
```

`PUT /jobs`
```json
{
  "title": "Desenvolvedor Front-End",
  "description": "asas asasa...",
  "place": "São Paulo",
  "salary": 1000000,
  "level": "junior",
  "stacks": ["html", "css", "javascript"],
  "type": "Remoto",
}
```

`DELETE /jobs/:vaga_id`

```
Não é necessário um corpo da requisição.
```

---
