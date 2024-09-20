### Construindo uma REST API com SOLID em Go - Parte 1: Estrutura, Server e Rotas

Neste artigo, vamos iniciar a criação de uma API RESTful seguindo os princípios SOLID, utilizando o framework Gin para o servidor HTTP, o GORM para o ORM, e SQLite como banco de dados. Nossa API será um CRUD de "Todo's", implementado com uma abordagem de Desenvolvimento Orientado a Testes (TDD).

#### Estrutura Inicial

Uma boa organização do projeto é essencial para seguir os princípios SOLID. Vamos começar definindo a estrutura de diretórios:

```
/go-todo-api
|-- /controllers      # Lida com a lógica das requisições HTTP
|-- /models           # Define as entidades e a interação com o banco de dados
|-- /services         # Implementa a lógica de negócios
|-- /routes           # Define as rotas da API
|-- /database         # Configura a conexão com o banco de dados
|-- main.go           # Arquivo principal que inicia o server
|-- go.mod            # Arquivo de dependências do Go
|-- go.sum            # Checksum das dependências
```

### Instalando o framework gin

Na pasta raíz do projeto, execute o comando abaixo para instalar o framework Gin:

```bash
go get -u github.com/gin-gonic/gin
```
#### Passo 3: Criando os Controladores

Os controladores vão lidar com a lógica das requisições HTTP. Para fins de teste, vamos criar os esboços dos controladores que serão implementados nas próximas partes do projeto.

**controllers/todo_controller.go**:
```go
package controllers

import (
	"net/http"
	"github.com/gin-gonic/gin"
)

// GetAllTodos retorna todos os todos
func GetAllTodos(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{"message": "Retorna todos os todos"})
}

// CreateTodo cria um novo todo
func CreateTodo(c *gin.Context) {
	c.JSON(http.StatusCreated, gin.H{"message": "Todo criado"})
}

// GetTodoByID retorna um todo pelo ID
func GetTodoByID(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{"message": "Retorna um todo pelo ID"})
}

// UpdateTodo atualiza um todo
func UpdateTodo(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{"message": "Todo atualizado"})
}

// DeleteTodo deleta um todo
func DeleteTodo(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{"message": "Todo deletado"})
}
```

#### Passo 2: Definindo as Rotas

Para seguir os princípios SOLID, vamos criar uma camada separada para as rotas. O arquivo `routes.go` será responsável por definir as rotas da API.

**routes/routes.go**:
```go
package routes

import (
	"go-todo-api/controllers"
	"github.com/gin-gonic/gin"
)

// InitializeRoutes inicializa todas as rotas da API
func InitializeRoutes(router *gin.Engine) {
	// Grupo de rotas de todo's
	todoRoutes := router.Group("/todos")
	{
		todoRoutes.GET("/", controllers.GetAllTodos)
		todoRoutes.POST("/", controllers.CreateTodo)
		todoRoutes.GET("/:id", controllers.GetTodoByID)
		todoRoutes.PUT("/:id", controllers.UpdateTodo)
		todoRoutes.DELETE("/:id", controllers.DeleteTodo)
	}
}
```

Aqui, criamos um grupo de rotas `/todos` que mapeia cada endpoint para um método específico no controlador.

#### Passo 3: Configurando o Servidor HTTP

Vamos começar criando o servidor HTTP usando o **Gin**.

**main.go**:
```go
package main

import (
	"go-todo-api/routes"
	"github.com/gin-gonic/gin"
)

func main() {
	// Inicializa o router Gin
	router := gin.Default()

	// Configura as rotas
	routes.InitializeRoutes(router)

	// Sobe o servidor na porta 8080
	router.Run(":8080")
}
```

#### Passo 4: Testando o Servidor e as Rotas

Com a estrutura, servidor e rotas configurados, vamos rodar o servidor e garantir que tudo está funcionando corretamente. Execute o comando abaixo no terminal:

```bash
go run main.go
```

Agora, abra o navegador ou use o `curl` para testar:

```bash
curl http://localhost:8080/todos
```

A resposta esperada será:

```json
{
  "message": "Retorna todos os todos"
}
```

#### Próximos Passos

Neste primeiro dia, criamos a estrutura do projeto, configuramos o servidor Gin e definimos as rotas da API. No próximo artigo, vamos configurar o banco de dados SQLite, integrar o GORM e começar a lidar com a persistência de dados.

Fique atento para a continuação, onde iremos avançar para a implementação das operações CRUD reais, aplicando conceitos de SOLID e TDD!

--- 

Nos vemos no próximo post!