pipeline {
  agent any

  tools {
    maven "M3"
    jdk "JDK21"
  }
  
  stages { 
    // github에 있는 소스코드 가져오기
    stage('Git Clone') {
      steps{
        git url: 'https://github.com/shinkang219/spring-petclinic-502.git/', 
        branch: 'main'
        
      }
    }
    //두번째 스테이지 maven 이용 빌드
    stage('Maven Build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      
      }
    
    }
    
  }
}
