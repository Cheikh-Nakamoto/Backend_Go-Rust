### Architecture du Projet

#### 1. Présentation de l'architecture
L'architecture est basée sur une API RESTful avec une séparation nette entre les différentes couches : présentation, logique métier, et accès aux données. 

#### 2. Composants principaux
1. **Serveur Backend** : Utilisation de Go avec le framework Gin.
2. **Base de données** : PostgreSQL pour la persistance des données.
3. **Tests** : Postman pour les tests d'API.
4. **Conteneurisation** : Docker pour faciliter le déploiement et la gestion des environnements.

#### 3. Schéma de l'architecture

```plaintext
+-----------------+          +------------------+         +----------------+
|  Client (HTTP)  | <------> |  Serveur Backend | <-----> |  Base de données |
+-----------------+          +------------------+         +----------------+
```

#### 4. Structure des répertoires
Organisez votre projet de manière à séparer les différentes responsabilités :

```plaintext
Backend_Go-Rust/
├── cmd/
│   └── main.go
├── config/
│   └── config.go
├── controllers/
│   └── task_controller.go
├── models/
│   └── task.go
├── repositories/
│   └── task_repository.go
├── routes/
│   └── task_routes.go
├── services/
│   └── task_service.go
├── utils/
│   └── response.go
├── Dockerfile
├── docker-compose.yml
├── go.mod
├── go.sum
└── README.md
```

#### 5. Description des répertoires et fichiers

1. **cmd/main.go** : Point d'entrée de l'application.
2. **config/config.go** : Configuration de l'application (base de données, variables d'environnement, etc.).
3. **controllers/task_controller.go** : Gestion des requêtes HTTP et réponses.
4. **models/task.go** : Définition des modèles de données (structs Go représentant les entités).
5. **repositories/task_repository.go** : Accès aux données, requêtes SQL.
6. **routes/task_routes.go** : Définition des routes/points d'entrée de l'API.
7. **services/task_service.go** : Logique métier.
8. **utils/response.go** : Fonctions utilitaires pour formater les réponses HTTP.
9. **Dockerfile** : Instructions pour construire l'image Docker de l'application.
10. **docker-compose.yml** : Configuration Docker Compose pour orchestrer les services (application et base de données).
11. **go.mod et go.sum** : Gestion des dépendances du projet Go.
12. **README.md** : Documentation du projet.

#### 6. Exemple de code

**cmd/main.go**
```go
package main

import (
    "Backend_Go-Rust/config"
    "Backend_Go-Rust/routes"
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    config.ConnectDatabase()
    routes.SetupRoutes(r)
    r.Run(":8080")
}
```

**config/config.go**
```go
package config

import (
    "gorm.io/driver/postgres"
    "gorm.io/gorm"
    "log"
)

var DB *gorm.DB

func ConnectDatabase() {
    dsn := "host=localhost user=postgres password=postgres dbname=task_management port=5432 sslmode=disable TimeZone=Asia/Shanghai"
    database, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})
    if err != nil {
        log.Fatal("Failed to connect to database:", err)
    }
    DB = database
}
```

**models/task.go**
```go
package models

import "time"

type Task struct {
    ID        uint      `gorm:"primaryKey"`
    Title     string    `json:"title"`
    Status    string    `json:"status"`
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}
```

**controllers/task_controller.go**
```go
package controllers

import (
    "Backend_Go-Rust/models"
    "Backend_Go-Rust/repositories"
    "Backend_Go-Rust/utils"
    "github.com/gin-gonic/gin"
    "net/http"
)

func GetTasks(c *gin.Context) {
    tasks, err := repositories.GetAllTasks()
    if err != nil {
        utils.RespondWithError(c, http.StatusInternalServerError, err.Error())
        return
    }
    utils.RespondWithJSON(c, http.StatusOK, tasks)
}

// Autres fonctions de CRUD (GetTask, CreateTask, UpdateTask, DeleteTask)
```

**routes/task_routes.go**
```go
package routes

import (
    "Backend_Go-Rust/controllers"
    "github.com/gin-gonic/gin"
)

func SetupRoutes(r *gin.Engine) {
    r.GET("/tasks", controllers.GetTasks)
    r.GET("/tasks/:id", controllers.GetTask)
    r.POST("/tasks", controllers.CreateTask)
    r.PUT("/tasks/:id", controllers.UpdateTask)
    r.DELETE("/tasks/:id", controllers.DeleteTask)
}
```

**repositories/task_repository.go**
```go
package repositories

import (
    "Backend_Go-Rust/config"
    "Backend_Go-Rust/models"
)

func GetAllTasks() ([]models.Task, error) {
    var tasks []models.Task
    if err := config.DB.Find(&tasks).Error; err != nil {
        return nil, err
    }
    return tasks, nil
}

// Autres fonctions de CRUD (GetTaskByID, CreateTask, UpdateTask, DeleteTask)
```

**utils/response.go**
```go
package utils

import "github.com/gin-gonic/gin"

func RespondWithError(c *gin.Context, code int, message string) {
    c.JSON(code, gin.H{"error": message})
}

func RespondWithJSON(c *gin.Context, code int, payload interface{}) {
    c.JSON(code, payload)
}
```

**Dockerfile**
```Dockerfile
FROM golang:1.16-alpine

WORKDIR /app

COPY go.mod ./
COPY go.sum ./
RUN go mod download

COPY . ./

RUN go build -o /backend_go_rust

EXPOSE 8080

CMD ["/backend_go_rust"]
```

**docker-compose.yml**
```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: task_management
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db

volumes:
  pgdata:
```