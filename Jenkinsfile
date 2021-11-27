pipeline {
  agent any
  stages{
        stage ('Init'){
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
        stage ('Build'){
          steps {
            echo "Build step..."
          }
        }
        stage ('Deploy'){
          steps {
            echo "Deploy step..."
          }
        }
    }
}