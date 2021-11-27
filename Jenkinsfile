pipeline {
  agent any
  stages{
        stage ('Build'){
          steps {
            sh 'mvn clean package' //скрипт запускающий maven-сборку нашего пакета
          }
          //активация нашего проекта, для этого добавили шаг пост-обработки
          post{
            success{
                echo 'Archiving...' //укажем, что при успешном завершении билда, выдать сообщение, что происходит архивация
                archiveArtifacts artifacts: '**/target/*.war' //И запаковать наш проект в файл war
            }
          }
        }
        stage ('Deploy to staging'){
          steps { // вызовем ранее созданную джобу, которая задеплоит наш пакет на тестовую среду
            build job: 'deploy_to_staging'
          }
        }
    }
}