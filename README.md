## 📄Documentação de APIs: Criação e Uso

### **🧩 O que é uma API?**

**API** significa **Application Programming Interface** (em português, Interface de Programação de Aplicações).

Ela funciona como um **mensageiro** entre dois sistemas — permitindo que eles conversem mesmo que tenham linguagens ou estruturas diferentes.

### **🧠Explicando com uma analogia:**
**Imagine que você está em um restaurante.**

1. Você (cliente) quer pedir um prato.

2. O garçom recebe seu pedido e o leva até a cozinha.

3. A cozinha prepara o prato e entrega ao garçom.

4. O garçom te traz o prato.

**Neste caso:**

 - Você é o sistema que faz a requisição.

 - A cozinha é o sistema que fornece os dados ou funcionalidades.

 - O garçom é a API — ele entrega o pedido, busca o resultado e traz de volta.

## **↔️Tipos Comuns de APIs**

 - **REST:**  ***(Representational State Transfer)*** é um **estilo de arquitetura** usado para construir **APIs** que se comunicam pela internet, especialmente usando o protocolo **HTTP** (o mesmo que usamos para acessar sites).

**🧪 Métodos HTTP usados no REST**
 | Método | Ação                      | Exemplo              | Ação |
| ------ | ------------------------- | -------------------- |------------|
| GET  | Buscar dados              | `GET /usuarios`    | Lista todos os usuários|
| POST | Criar novo recurso        | `POST /usuarios`   | Cria um novo usuário |           
| PUT  | Atualizar recurso inteiro | `PUT /usuarios/10` |	Atualiza todos os dados do usuário 10|
| PATCH  | Atualiza parte do recurso    | `PATCH /usuarios/10`  | Quando você quer alterar só um campo |
| DELETE | Remover recurso           | `DELETE /usuarios/10` | Exclui o usuário com ID 10|
|



 - **SOAP (Simple Object Access Protocol):** Significa **(Protocolo Simples de Acesso a Objetos)** é um protocolo usado para trocar mensagens estruturadas entre sistemas pela internet, geralmente em formato XML.






 - **GraphQL:** Permite que o cliente especifique exatamente quais dados deseja.








 - **Webhooks:** APIs baseadas em eventos, onde o servidor envia dados ao cliente quando algo ocorre.







3. Como Criar uma API
Criar uma API envolve planejar sua estrutura, escolher tecnologias adequadas e implementar endpoints que atendam às necessidades do projeto. A seguir, um guia passo a passo para criar uma API RESTful simples usando Python e Flask.
3.1. Planejamento

Defina o Propósito: Determine o que a API fará (ex.: gerenciar usuários, fornecer dados de produtos).
Estruture os Endpoints:
/users: Listar todos os usuários (GET) ou criar um novo usuário (POST).
/users/{id}: Obter, atualizar ou excluir um usuário específico (GET, PUT, DELETE).


Escolha o Formato de Dados: JSON é o padrão mais comum para APIs REST.
Planeje a Autenticação: Use tokens (JWT, OAuth) ou chaves de API para segurança.

3.2. Tecnologias Sugeridas

Linguagem: Python (Flask, FastAPI), Node.js (Express), Java (Spring Boot).
Banco de Dados: MySQL, PostgreSQL, MongoDB.
Ferramentas de Teste: Postman, Insomnia.
Documentação: Swagger/OpenAPI, Redoc.

3.3. Exemplo de Implementação (Python + Flask)
Abaixo está um exemplo de uma API REST simples usando Flask para gerenciar uma lista de tarefas.
Código da API
from flask import Flask, request, jsonify

app = Flask(__name__)

## Banco de dados simulado (lista em memória)
tasks = [
    {"id": 1, "title": "Comprar leite", "done": False},
    {"id": 2, "title": "Estudar Python", "done": True}
]

### Obter todas as tarefas
@app.route('/tasks', methods=['GET'])
def get_tasks():
    return jsonify(tasks)

### Obter uma tarefa específica
@app.route('/tasks/<int:task_id>', methods=['GET'])
def get_task(task_id):
    task = next((task for task in tasks if task['id'] == task_id), None)
    if task:
        return jsonify(task)
    return jsonify({"error": "Tarefa não encontrada"}), 404

### Criar uma nova tarefa
@app.route('/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    if not data or not data.get('title'):
        return jsonify({"error": "Título é obrigatório"}), 400
    new_task = {
        "id": len(tasks) + 1,
        "title": data['title'],
        "done": data.get('done', False)
    }
    tasks.append(new_task)
    return jsonify(new_task), 201

### Atualizar uma tarefa
@app.route('/tasks/<int:task_id>', methods=['PUT'])
def update_task(task_id):
    task = next((task for task in tasks if task['id'] == task_id), None)
    if not task:
        return jsonify({"error": "Tarefa não encontrada"}), 404
    data = request.get_json()
    task['title'] = data.get('title', task['title'])
    task['done'] = data.get('done', task['done'])
    return jsonify(task)

### Excluir uma tarefa
@app.route('/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    global tasks
    tasks = [task for task in tasks if task['id'] != task_id]
    return jsonify({"message": "Tarefa excluída"})

if __name__ == '__main__':
    app.run(debug=True)

Explicação do Código

Flask: Framework leve para criar APIs em Python.
Endpoints:
GET /tasks: Retorna todas as tarefas.
GET /tasks/<id>: Retorna uma tarefa específica.
POST /tasks: Cria uma nova tarefa.
PUT /tasks/<id>: Atualiza uma tarefa existente.
DELETE /tasks/<id>: Exclui uma tarefa.


Formato de Resposta: JSON, com códigos de status HTTP apropriados (200, 201, 404, etc.).

3.4. Testando a API

Execute o código acima em um ambiente Python com Flask instalado (pip install flask).
Use ferramentas como Postman ou curl para testar os endpoints:curl http://localhost:5000/tasks
curl -X POST http://localhost:5000/tasks -H "Content-Type: application/json" -d '{"title": "Nova tarefa"}'



3.5. Documentando a API
Use ferramentas como Swagger/OpenAPI para criar uma documentação interativa. Um exemplo de especificação OpenAPI para a API de tarefas:
openapi: 3.0.0
info:
  title: API de Gerenciamento de Tarefas
  version: 1.0.0
paths:
  /tasks:
    get:
      summary: Lista todas as tarefas
      responses:
        '200':
          description: Lista de tarefas
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Task'
    post:
      summary: Cria uma nova tarefa
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskInput'
      responses:
        '201':
          description: Tarefa criada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: integer
        title:
          type: string
        done:
          type: boolean
    TaskInput:
      type: object
      properties:
        title:
          type: string
        done:
          type: boolean

Salve o arquivo acima como openapi.yaml e use ferramentas como Swagger UI para visualizá-lo.

4. Como Usar uma API
4.1. Fazendo Requisições
Para consumir uma API, você precisa entender seus endpoints, métodos HTTP e formato de dados. Ferramentas como Postman, Insomnia ou bibliotecas como requests (Python) são úteis.
Exemplo em Python
import requests

### Obter todas as tarefas
response = requests.get('http://localhost:5000/tasks')
print(response.json())

### Criar uma nova tarefa
new_task = {"title": "Aprender APIs", "done": False}
response = requests.post('http://localhost:5000/tasks', json=new_task)
print(response.json())

4.2. Autenticação
Muitas APIs requerem autenticação:

Chave de API: Incluída no cabeçalho ou como parâmetro (ex.: ?api_key=xyz).
JWT: Token enviado no cabeçalho Authorization: Bearer <token>.
OAuth: Fluxo de autenticação para acessar APIs de terceiros.

4.3. Tratamento de Erros
Verifique os códigos de status HTTP:


| Código HTTP | Significado                 |
|-------------|-----------------------------|
| 200 OK      | Sucesso.                    |
| 201 Created | Recurso criado.             |
| 400 Bad Request | Erro na solicitação do cliente. |
| 401 Unauthorized | Autenticação necessária.       |
| 404 Not Found | Recurso não encontrado.          |
| 500 Internal Server Error | Erro no servidor.     |
|


5. Boas Práticas para APIs

Versionamento: Use /v1/ nos endpoints para suportar atualizações futuras.
Documentação Clara: Forneça exemplos de requisições e respostas.
Segurança:
Use HTTPS para criptografia.
Implemente autenticação e autorização.
Valide todas as entradas do usuário.


Paginação: Para grandes conjuntos de dados, use parâmetros como ?page=1&limit=10.
Cache: Use cabeçalhos como ETag ou Cache-Control para melhorar a performance.
Consistência: Siga padrões como REST ou GraphQL de forma consistente.


6. Ferramentas Úteis

Desenvolvimento:
Flask, FastAPI (Python)
Express (Node.js)
Spring Boot (Java)


Teste:
Postman
Insomnia
cURL


Documentação:
Swagger/OpenAPI
Redoc


Monitoramento:
Postman Monitors
New Relic
Grafana




7. Conclusão
Criar e usar APIs requer planejamento cuidadoso, implementação robusta e documentação clara. APIs bem projetadas facilitam a integração entre sistemas e melhoram a experiência do desenvolvedor. Para começar, experimente criar uma API simples como a do exemplo acima e explore ferramentas como Swagger para documentação.
Se precisar de mais detalhes ou exemplos, consulte recursos como a documentação oficial do Flask ou a especificação OpenAPI.