pipeline {
  agent any

  tools {
    maven "M3"
    jdk "JDK21"
  }

  environment {
    DOCKERHUB_CRED = credentials('dockerCredentials')
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
    //docker 이미지 생성
    stage('Docker Image Create') {
      steps {
        sh '''
          docker build -t spring-petclinic:${BUILD_NUMBER} .
          docker tag spring-petclinic:${BUILD_NUMBER} darararara/spring-petclinic:latest
         
        '''
     
      }
    }

    //docker image push

    stage('Docker Image Push') {
      steps {
        sh '''
          echo ${DOCKERHUB_CRED_PSW} | docker login -u ${DOCKERHUB_CRED_USR} --password-stdin
          docker push darararara/spring-petclinic:latest
        '''
      }
    }

    
    
    //ssh를 이용한 전송및 실행
    stage('SSH Publish') {
      steps {        
        sshPublisher(publishers: [sshPublisherDesc(configName: 'target', 
        transfers: [sshTransfer(cleanRemote: false, 
        excludes: '', 
                                
        execCommand: '''
        docker rm -f $(docker ps -aq)
        docker rmi -f $(docker images -q)
        docker run -itd -p 80:8080 --name=spring-petclinic darararara/spring-petclinic:latest
        ''', 
                                
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
    // Docker image 삭제
    stage('Docker Image Remove') {
      steps {
        sh '''
          docker rmi -f spring-petclinic:${BUILD_NUMBER}
          docker rmi -f darararara/spring-petclinic:latest
        '''
      }
    }
  }
}
