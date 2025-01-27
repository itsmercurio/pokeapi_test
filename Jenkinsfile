pipeline {
    agent any  // Usar cualquier agente disponible de Jenkins

    environment {
        DOCKER_IMAGE = 'pokeapi-app:latest'  // Nombre para la imagen Docker
        CONTAINER_NAME = 'pokeapi-container'  // Nombre para el contenedor Docker
    }

    stages {
        // 1. Checkout: Obtener el código del repositorio
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Jillazquez/pokeApi.git', branch: 'main'  // Clonar el repositorio
            }
        }

        // 2. Build Docker Image: Crear la imagen Docker con el Dockerfile
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    bat 'docker build -t %DOCKER_IMAGE% .'  // Construir imagen
                }
            }
        }

        // 3. Run Docker Container: Ejecutar el contenedor con la imagen construida
        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Running Docker container...'
                    bat 'docker run -d --name %CONTAINER_NAME% -p 8000:8000 %DOCKER_IMAGE%'  // Iniciar contenedor
                }
            }
        }

        // 4. Run Tests: Ejecutar los tests en el contenedor
        stage('Run Pytest') {
            steps {
                script {
                    echo 'Running Pytest...'
                    bat 'docker exec %CONTAINER_NAME% /app/venv/Scripts/pytest tests/'  // Ejecutar pytest
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'  // Si la ejecución es exitosa
        }
        failure {
            echo 'There was an issue with the pipeline.'  // Si ocurre algún error
        }
    }
}
