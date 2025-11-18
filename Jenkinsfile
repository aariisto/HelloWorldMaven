pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/aariisto/HelloWorldMaven'

                // Run Maven on a Unix agent.
                sh "mvn clean install"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    script {
            // On récupère les identifiants stockés dans Jenkins
            // Remplace 'github-access' par l'ID de tes credentials dans Jenkins
            withCredentials([usernamePassword(credentialsId: 'f0239823-d0de-437c-b53f-2f1ba19dc971', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                
                sh """
                
                # 1. Configurer l'identité pour ce commit (obligatoire pour git tag)
                    git config user.email "yannelaissani@gmail.com"
                    git config user.name "aariisto"


                    # 2. Créer le tag localement
                    # L'option -f (force) permet d'écraser si le tag existe déjà en local
                    git tag -f -a "v2.${BUILD_NUMBER}" -m "Version livrée par Jenkins"

                    # 3. Pousser le tag vers GitHub
                    # On insère le user/pass dans l'URL de manière sécurisée
                    git push https://${GIT_USER}:${GIT_PASS}@github.com/aariisto/HelloWorldMaven.git "v2.${BUILD_NUMBER}"
                """
            }
        }
                }
            }
        }
    }
}
