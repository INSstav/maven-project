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
        stage ('Deploy to prod'){
          steps {
            //эта функция задаёт ожидание в 5 дней подтверждения от пользователя перед запуском джобы
            timeout(time:5, unit:'DAYS') {
                //если в эти 5 дней никто не нажмёт кнопку подтверждения, то джоба выполнится автоматически
                input message: 'Approve prod deployment?'
            }
            build job: 'deploy_to_prod' // вызовем ранее созданную джобу, которая задеплоит наш пакет на проакшен
          }
        }
    }
}