## 3 - Sua aplicação deve ter o endpoint POST `/login`

- O endpoint deve ser acessível através do URL `/login`;
- O corpo da requisição deverá seguir o formato abaixo:
  ```json
  {
    "email": "lewishamilton@gmail.com",
    "password": "123456"
  }
  ```

<details>
  <summary><strong>Os seguintes pontos serão avaliados:</strong></summary>

  * **[Será validado que não é possível fazer login sem todos os campos preenchidos]**
    - Se a requisição não tiver todos os campos devidamente preenchidos(não pode haver campos em branco), o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "Some required fields are missing"
    }
    ```

  * **[Será validado que não é possível fazer login com um usuário que não existe]**
    - Se a requisição receber um par de `email` e `password` errados/inexistentes, o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "Invalid fields"
    }
    ```
  
  * **[Será validado que é possível fazer login com sucesso]**
    - Se o login foi feito com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `200`:
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXlsb2FkIjp7ImlkIjo1LCJkaXNwbGF5TmFtZSI6InVzdWFyaW8gZGUgdGVzdGUiLCJlbWFpbCI6InRlc3RlQGVtYWlsLmNvbSIsImltYWdlIjoibnVsbCJ9LCJpYXQiOjE2MjAyNDQxODcsImV4cCI6MTYyMDY3NjE4N30.Roc4byj6mYakYqd9LTCozU1hd9k_Vw5IWKGL4hcCVG8"
    }
    ```
    > :warning: O token anterior é fictício, seu token deve ser gerado a partir da variável de ambiente `JWT_SECRET`, do `payload` da requisição e não deve conter o atributo `password` em sua construção.

<br />
</details>

---

## 4 - Sua aplicação deve ter o endpoint POST `/user`

- O endpoint deve ser acessível através do URL `/user`;
- O endpoint deve ser capaz de adicionar um novo `user` a sua tabela no banco de dados;
- O corpo da requisição deverá seguir o formato abaixo:
  ```json
  {
    "displayName": "Brett Wiltshire",
    "email": "brett@email.com",
    "password": "123456",
    "image": "http://4.bp.blogspot.com/_YA50adQ-7vQ/S1gfR_6ufpI/AAAAAAAAAAk/1ErJGgRWZDg/S45/brett.png"
  }
  ```

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * **[Será validado que não é possível cadastrar com o campo `displayName` menor que 8 caracteres]**
    - Se a requisição não tiver o campo `displayName` devidamente preenchido com 8 caracteres ou mais, o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "\"displayName\" length must be at least 8 characters long"
    }
    ```
  
  * **[Será validado que não é possível cadastrar com o campo `email` com formato inválido]**
    - Se a requisição não tiver o campo `email` devidamente preenchido com o formato `<prefixo@dominio>`, o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "\"email\" must be a valid email"
    }
    ```

  * **[Será validado que não é possível cadastrar com o campo `password` menor que 6 caracteres]**
    - Se a requisição não tiver o campo `password` devidamente preenchido com 6 caracteres ou mais, o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "\"password\" length must be at least 6 characters long"
    }
    ```

  * **[Será validado que não é possível cadastrar com um email já existente]**
    - Se a requisição enviar o campo `email` com um email que já existe, o resultado retornado deverá ser conforme exibido abaixo, com um status http `409`:
    ```json
    {
      "message": "User already registered"
    }
    ```
  
  * **[Será validado que é possível cadastrar um pessoa usuária com sucesso]**
    - Se o user for criado com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `201`:
    ```json
      {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXlsb2FkIjp7ImlkIjo1LCJkaXNwbGF5TmFtZSI6InVzdWFyaW8gZGUgdGVzdGUiLCJlbWFpbCI6InRlc3RlQGVtYWlsLmNvbSIsImltYWdlIjoibnVsbCJ9LCJpYXQiOjE2MjAyNDQxODcsImV4cCI6MTYyMDY3NjE4N30.Roc4byj6mYakYqd9LTCozU1hd9k_Vw5IWKGL4hcCVG8"
      }
      ```
    > :warning: O token anterior é fictício, seu token deve ser gerado a partir da variável de ambiente `JWT_SECRET`, do `payload` da requisição e não deve conter o atributo `password` em sua construção.

<br />
</details>

---

## :warning: Validando token nas requisições

- Após termos feito o requisito de criação de `users` e o requisito de `login`, alguns requisitos abaixo vão precisar desta autenticação prévia, para que seja possível consumir o endpoint;
- Todo requisito que precisar validar o `token` terá o símbolo ☝;
- **✨ Dica:** Se é algo que vamos utilizar em mais de uma rota, será que podemos separa-lo em algum lugar que comece com `M` de `middleware`? 😜

<details>
  <summary id="validandoToken"><strong>Os seguintes pontos serão avaliados</strong></summary>

  * **[Será validado que não é possível fazer uma operação sem o token na requisição]**
    - Se o token for inexistente o resultado retornado deverá ser conforme exibido abaixo, com um status http `401`:
    ```json
    {
      "message": "Token not found"
    }
    ```

  * **[Será validado que não é possível fazer uma operação com o token inválido]**
    - Se o token for inválido o resultado retornado deverá ser conforme exibido abaixo, com um status http `401`:
    ```json
    {
      "message": "Expired or invalid token"
    }
    ```

</details>

---

## 5 - Sua aplicação deve ter o endpoint GET `/user`

- ☝ Não esqueça de validar o `token` neste requisito, como descrito na seção de [Validando token nas requisições](#validandoToken);
- O endpoint deve ser acessível através do URL `/user`;
- O endpoint deve ser capaz de trazer todos `users` do banco de dados;

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * ☝ **[Será validado o token, como descrito na seção de [Validando token nas requisições](#validandoToken)]**

  * **[Será validado que é possível listar todos os usuários]**
    - Ao listar usuários com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `200`:
    ```json
    [
      {
          "id": 1,
          "displayName": "Lewis Hamilton",
          "email": "lewishamilton@gmail.com",
          "image": "https://upload.wikimedia.org/wikipedia/commons/1/18/Lewis_Hamilton_2016_Malaysia_2.jpg"
      },

      /* ... */
    ]
    ```

<br />
</details>

---

## 6 - Sua aplicação deve ter o endpoint GET `/user/:id`

- ☝ Não esqueça de validar o `token` neste requisito, como descrito na seção de [Validando token nas requisições](#validandoToken);
- O endpoint deve ser acessível através do URL `/user/:id`;
- O endpoint deve ser capaz de trazer o `user` baseado no `id` do banco de dados se ele existir;

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * ☝ **[Será validado o token, como descrito na seção de [Validando token nas requisições](#validandoToken)]**

  * **[Será validado que é possível listar um usuário específico com sucesso]**
    - Ao listar um usuário com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `200`:
    ```json
    {
      "id": 1,
      "displayName": "Lewis Hamilton",
      "email": "lewishamilton@gmail.com",
      "image": "https://upload.wikimedia.org/wikipedia/commons/1/18/Lewis_Hamilton_2016_Malaysia_2.jpg"
    }
    ```

  * **[Será validado que não é possível listar um usuário inexistente]**
    - Se o usuário for inexistente o resultado retornado deverá ser conforme exibido abaixo, com um status http `404`:
    ```json
    {
      "message": "User does not exist"
    }
    ```

<br />
</details>

---

## 8 - Sua aplicação deve ter o endpoint POST `/categories`

- ☝ Não esqueça de validar o `token` neste requisito, como descrito na seção de [Validando token nas requisições](#validandoToken);
- O endpoint deve ser acessível através do URL `/categories`;
- O endpoint deve ser capaz de adicionar uma nova categoria a sua tabela no banco de dados;
- O corpo da requisição deverá seguir o formato abaixo:
  ```json
  {
    "name": "Typescript"
  }
  ```

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * ☝ **[Será validado o token, como descrito na seção de [Validando token nas requisições](#validandoToken)]**

  * **[Será validado que não é possível cadastrar uma categoria sem o campo `name`]**
    - Se a requisição não tiver o campo `name` devidamente preenchidos(não pode haver campo em branco), o resultado retornado deverá ser conforme exibido abaixo, com um status http `400`:
    ```json
    {
      "message": "\"name\" is required"
    }
    ```

  * **[Será validado que é possível cadastrar uma categoria com sucesso]**
    - Se a categoria for criada com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `201`:
    ```json
    {
      "id": 3,
      "name": "Typescript"
    }
    ```

<br />
</details>

---

## 9 - Sua aplicação deve ter o endpoint GET `/categories`

- ☝ Não esqueça de validar o `token` neste requisito, como descrito na seção de [Validando token nas requisições](#validandoToken);
- O endpoint deve ser acessível através do URL `/categories`;
- O endpoint deve ser capaz de trazer todas categorias do banco de dados;

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * ☝ **[Será validado o token, como descrito na seção de [Validando token nas requisições](#validandoToken)]**

  * **[Será validado que é possível listar todas as categoria com sucesso]**
    - Ao listar categorias com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `200`:
    ```json
    [
      {
          "id": 1,
          "name": "Inovação"
      },
      {
          "id": 2,
          "name": "Escola"
      },

      /* ... */
    ]
    ```

<br />
</details>

---

## 13 - Sua aplicação deve ter o endpoint GET `/post`

- ☝ Não esqueça de validar o `token` neste requisito, como descrito na seção de [Validando token nas requisições](#validandoToken);
- O endpoint deve ser acessível através do URL `/post`;
- O endpoint deve ser capaz de trazer todos os bogs post, user dono dele e as categorias do banco de dados;

<details>
  <summary><strong>Os seguintes pontos serão avaliados</strong></summary>

  * ☝ **[Será validado o token, como descrito na seção de [Validando token nas requisições](#validandoToken)]**

  * **[Será validado que é possível listar blogpost com sucesso]**
    - Ao listar posts com sucesso o resultado retornado deverá ser conforme exibido abaixo, com um status http `200`:
    ```json
    [
      {
        "id": 1,
        "title": "Post do Ano",
        "content": "Melhor post do ano",
        "userId": 1,
        "published": "2011-08-01T19:58:00.000Z",
        "updated": "2011-08-01T19:58:51.000Z",
        "user": {
          "id": 1,
          "displayName": "Lewis Hamilton",
          "email": "lewishamilton@gmail.com",
          "image": "https://upload.wikimedia.org/wikipedia/commons/1/18/Lewis_Hamilton_2016_Malaysia_2.jpg"
        },
        "categories": [
          {
            "id": 1,
            "name": "Inovação"
          }
        ]
      },
      
      /* ... */
    ]
    ```

<br />
</details>

---

