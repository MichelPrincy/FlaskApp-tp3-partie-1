pipeline {
    agent any // Utilise l'environnement Jenkins Docker de base

    triggers {
        pollSCM('*/1 * * * *') // Alerte Jenkins toutes les minutes en cas de push GitHub
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm // Étape 1 : Jenkins clone ton dépôt
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Exécution des tests unitaires dans le conteneur...'
                // On build temporairement l'image pour exécuter les tests à l'intérieur
                sh "docker build -t flask_hello ."
                // On lance les tests en écrasant la commande par défaut (Page 6 du TP)
                sh "docker run --rm --name flask_hello flask_hello ./test.py --verbose"
            }
        }

        stage('Push to Local Registry') {
            steps {
                echo 'Tag et Push de l\'image vers le registre local...'
                sh "docker tag flask_hello localhost:4000/flask_hello"
                sh "docker push localhost:4000/flask_hello"
            }
        }
    }
}