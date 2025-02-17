## **1️⃣ Rodar o Keycloak com Docker**
Execute o comando abaixo para iniciar o **Keycloak** na porta **8080**:

```sh
docker run -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:22.0.5 start-dev
```

Isso irá iniciar o Keycloak no modo **desenvolvimento**, sem necessidade de um banco externo.

---

## **2️⃣ Criar um Realm no Keycloak**
1. Acesse **http://localhost:8080/**  
2. Faça login com **Usuário:** `admin` e **Senha:** `admin`  
3. Clique em **"Create Realm"** (Criar Realm)  
4. Nomeie o realm como:  
   - **Realm Name:** `meu-realm`  
5. Clique em **"Create"**  

---

## **3️⃣ Criar um Usuário no Keycloak**
1. No menu lateral, vá em **Users** → **Add User**  
2. Preencha:  
   - **Username:** `meu-usuario`  
3. Clique em **"Create"**  
4. Vá para a aba **"Credentials"** e defina uma senha:  
   - **Password:** `123456`  
   - **Temporary:** ❌ **(Desmarque para não expirar)**  
5. Clique em **"Set Password"**  

---

## **4️⃣ Criar um Client no Keycloak**
1. Vá para **Clients** → **Create Client**  
2. Preencha:  
   - **Client ID:** `gateway`  
   - **Client Type:** `OpenID Connect`  
3. Clique em **"Next"**  
4. Na aba **Settings**, configure:  
   - **Client Authentication:** ✅ (Ativar)  
   - **Authorization Enabled:** ✅ (Ativar)  
   - **Direct Access Grants:** ❌ (Desativar)  
   - **Valid Redirect URIs:** `http://localhost:9002/*`  
5. Clique em **Save**  

### **4.1 Gerar Client Secret**
1. Vá para a aba **Credentials**  
2. Copie o valor do **Client Secret** gerado  
3. Guarde essa senha para configurar a aplicação  

---

## **5️⃣ Criar as Variáveis de Ambiente**
Agora, crie um arquivo `.env` na raiz do projeto ou exporte as variáveis manualmente:

```
CLIENT_ID=gateway
CLIENT_SECRET=<CLIENT_SECRET_COPIADO_DO_KEYCLOAK>
KEYCLOAK_REALM=meu-realm
```

Se estiver no terminal, exporte as variáveis manualmente:

```sh
export CLIENT_ID=gateway
export CLIENT_SECRET=<CLIENT_SECRET_COPIADO_DO_KEYCLOAK>
export KEYCLOAK_REALM=meu-realm
```

---

## **6️⃣ Rodar o Resource Server**
Agora, execute o **Resource Server** para rodar na porta **8081**.

```sh
cd resource-server
mvn spring-boot:run
```

Verifique se ele está rodando em `http://localhost:8081/`.

---

## **7️⃣ Rodar o API Gateway**
Agora, execute o **Gateway** para rodar na porta **9002**.

```sh
cd gateway
mvn spring-boot:run
```

Verifique se ele está rodando em `http://localhost:9002/`.

---

## **8️⃣ Testar a Autenticação**
Agora, tente acessar `http://localhost:9002/hello` e veja se o Keycloak redireciona para login.

### **Passo a passo para testar no navegador**
1. Acesse `http://localhost:9002/hello`
2. O navegador redirecionará para a tela de login do **Keycloak**
3. Faça login com:  
   - **Usuário:** `meu-usuario`  
   - **Senha:** `123456`  
4. Após login, o navegador deve redirecionar para o `hello` do **Resource Server**, retornando uma resposta.  

Se tudo funcionou, significa que:  
✅ O **Keycloak autenticou o usuário**  
✅ O **Gateway encaminhou o token**  
✅ O **Resource Server validou o token e respondeu corretamente**  

---

## **Conclusão**
Agora você tem um ambiente **Keycloak + API Gateway + Resource Server** rodando com segurança OAuth2! 🚀  

