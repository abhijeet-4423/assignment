pipeline{
    agent {
        label {
            label "slave-1"
            customWorkspace "/mnt/"
        }
    }
    stages {
        stage ("parallel"){
            parallel{
                
                stage ("slave-1"){
                    agent {
                        label {
                            label "slave-1"
                            customWorkspace "/mnt/git/"
                        }
                    }    
                    steps {
                        sh "rm -rf /mnt/git/*"
                        sh "git clone https://github.com/abhijeet-4423/game-of-life.git"
                        dir ("/mnt/git/game-of-life/"){
                            sh "mvn clean install"
                        }
                        sh "cp /mnt/git/game-of-life/gameoflife-web/target/gameoflife.war /mnt/apache-tomcat-9.0.71/webapps/"
                    }
                }
                stage ("slave-2"){
                    agent {
                        label {
                            label "slave-2"
                            customWorkspace "/mnt/git/"
                        }
                    }    
                    steps {
                        sh "rm -rf /mnt/git/*"
                        sh "git clone https://github.com/abhijeet-4423/game-of-life.git"
                        dir ("/mnt/git/game-of-life/"){
                            sh "mvn clean install"
                        }
                        sh "cp /mnt/git/game-of-life/gameoflife-web/target/gameoflife.war /mnt/apache-tomcat-9.0.71/webapps/"
                    }
                }
            }
        }
    }
}
