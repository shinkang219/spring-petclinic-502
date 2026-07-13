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
    //ssh를 이용한 전송및 실행
    stage('SSH Publish') {
      steps {        
        sshPublisher(publishers: [sshPublisherDesc(configName: 'target', 
        transfers: [sshTransfer(cleanRemote: false, 
        excludes: '', 
        execCommand: '''fuser -k 8080/tcp
        export BUILD_ID=PetClinic
        
        nohup java -jar /home/ubuntu/spring-petclinic-4.0.0-SNAPSHOT.jar >> nohup.out 2>&1 &''', 
        execTimeout: 120000, 
        flatten: false, 
        makeEmptyDirs: false, 
        noDefaultExcludes: false, 
        patternSeparator: '[, ]+', 
        remoteDirectory: '', 
        remoteDirectorySDF: false, 
        removePrefix: 'target', 
        sourceFiles: 'target/spring-petclinic-4.0.0-SNAPSHOT.jar')], 
        usePromotionTimestamp: false, 
        useWorkspaceInPromotion: false, 
        verbose: false)])
      }
    
    }
  }
}
