pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    stages {
        stage("Checkout") {
            steps {
                git 'https://github.com/Chinku-code/webapp.git'
            }
        }

        stage("Compile") {
            steps {
                sh 'mvn compile'
            }
        }

        stage("Test") {
            steps {
                sh 'mvn test'
            }
        }

        stage("Package") {
            steps {
                sh 'mvn clean package'
                sh 'mv target/*.war target/myweb.war'
            }
        }

        stage("Deploy") {
            steps {
                sshagent(['tomcat-creds']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@15.206.178.27:/home/ec2-user/tomcat-10/webapps/
                        ssh -o StrictHostKeyChecking=no ec2-user@15.206.178.27 "bash /home/ec2-user/tomcat-10/bin/shutdown.sh"
                        ssh -o StrictHostKeyChecking=no ec2-user@15.206.178.27 "bash /home/ec2-user/tomcat-10/bin/startup.sh"
                    """
                }
            }
        }
    }
}
