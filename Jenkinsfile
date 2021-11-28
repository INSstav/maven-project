pipeline {
  agent any
  // триггер - проверка кода в репозитории на изменение
  triggers{
    pollSCM('*/5 * * * *')  //будет раз в 5 минут проверять код в репозитории на изменение (синтаксис Cron)
  }
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
        stage ('Deployments'){
            parallel { //Оба деплоя будут выполняться параллельно
                stage ('Deploy to staging'){
                  steps {
                    sh "ch **/target/*.war /home/ivan_sapronov/programs/tomcat-staging/webapps"
                    // build job: 'deploy_to_staging' // вызовем ранее созданную джобу
                  }
                }
                stage ('Deploy to prod'){
                  steps {
//                     //эта функция задаёт ожидание в 5 дней подтверждения от пользователя перед запуском джобы
//                     timeout(time:5, unit:'DAYS') {
//                         //если в эти 5 дней никто не нажмёт кнопку подтверждения, то джоба выполнится автоматически
//                         input message: 'Approve prod deployment?'
//                     }
                    sh "ch **/target/*.war /home/ivan_sapronov/programs/tomcat-prod/webapps"
                    //build job: 'deploy_to_prod' // вызовем ранее созданную джобу, которая задеплоит наш пакет на проакшен
                  }
                }
            }
        }

//**************************************************************************************************************
// Первый вариант - Деплой в прод происходит после деплоя в тестовую среду
//
//         stage ('Deploy to staging'){
//           steps { // вызовем ранее созданную джобу, которая задеплоит наш пакет на тестовую среду
//             build job: 'deploy_to_staging'
//           }
//         }
//         stage ('Deploy to prod'){
//           steps {
//             //эта функция задаёт ожидание в 5 дней подтверждения от пользователя перед запуском джобы
//             timeout(time:5, unit:'DAYS') {
//                 //если в эти 5 дней никто не нажмёт кнопку подтверждения, то джоба выполнится автоматически
//                 input message: 'Approve prod deployment?'
//             }
//             build job: 'deploy_to_prod' // вызовем ранее созданную джобу, которая задеплоит наш пакет на проакшен
//           }
//         }
//***************************************************************************************************************
    }
}