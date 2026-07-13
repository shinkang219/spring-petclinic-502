pipeline {
  agent any

  tools {
    maven "M3"
    jdk "JDK21"
  }
  
  stages { 
    // github에 있는 소스코드 가져오기
    stage(Git Clone) {
      steps{
        git url: 'https://github.com/shinkang219/spring-petclinic-502.git/', 
        branch: 'main'
      }
    }
  }
}
