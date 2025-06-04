# 📂 Repositórios e Checkpoints

---

# Disclaimer
Os repositórios listados abaixo foram usados no meu projeto individual, no projeto os repositórios utilizados foram os do Eduardo Takei. Na aba do projeto isto está documentado, porém queria deixar claro que os repositórios abaixo são os utilizados apenas no meu projeto individual e não no projeto em grupo.

---

## 🔗 Lista de Repositórios Utilizados


### 1. [**account**](https://github.com/LucaFeltrin14/account)

---

### 2. [**account-service**](https://github.com/LucaFeltrin14/account-service)

---

### 3. [**auth**](https://github.com/LucaFeltrin14/auth)

---

### 4. [**auth-service**](https://github.com/LucaFeltrin14/auth-service)

---

### 5. [**gateway-service**](https://github.com/LucaFeltrin14/gateway-service)

---

### 6. [**exchange-service**](https://github.com/LucaFeltrin14/exchange-service)

---

### 7. [**product**](https://github.com/LucaFeltrin14/product)

---

### 8. [**product-service**](https://github.com/LucaFeltrin14/product-service)

---

### 9. [**order**](https://github.com/LucaFeltrin14/order)

---

### 10. [**order-service**](https://github.com/LucaFeltrin14/order-service)


## Instruções de Execução

### 1. Clonar o repositório principal

```bash
git clone https://github.com/LucaFeltrin14/platfp.git
cd plataforma
cd api
```

### 2. Subir os serviços com Docker Compose

Certifique-se de ter o Docker e o Docker Compose instalados:

```bash
docker compose up --build
```

Esse comando irá construir todas as imagens e subir os containers definidos no `compose.yml`.

### 3. Acessar a aplicação

Após os containers estarem rodando, você pode acessar a aplicação via gateway:

```
http://localhost:8080
```

Exemplos de rotas disponíveis:

- `POST /auth/register` – Registro de usuários
- `POST /auth/login` – Login de usuários
- `GET /account/{userId}` – Obter detalhes da conta do usuário
- `GET /product` – Listar todos os produtos
- `POST /order` – Criar um novo pedido

### 4. Encerrar os serviços

Para parar todos os containers e liberar os recursos:

```bash
docker compose down
```

---

