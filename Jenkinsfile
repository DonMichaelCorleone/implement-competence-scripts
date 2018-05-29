pipeline {
  // agent defines where the pipeline will run ..
  agent {
    // This also could have been 'agent any' - that has the same meaning.
    label "XPOSI"
    // Other possible built-in agent types are 'agent none', for not running the
    // top-level on any agent (which results in you needing to specify agents on
    // each stage and do explicit checkouts of scm in those stages), 'docker',
    // and 'dockerfile'.
  }
  environment {
    // Environment variable identifiers need to be both valid bash variable
    // identifiers and valid Groovy variable identifiers. If you use an invalid
    // identifier, you'll get an error at validation time.
    // Right now, you can't do more complicated Groovy expressions or nesting of
    // other env vars in environment variables values, but that will be possible
    // when https://issues.jenkins-ci.org/browse/JENKINS-41748 is merged and
    // released.
    JENKINS_MAIL_ADRESS='jenkinssquare@gmail.com'
  }
  stages {
    stage('SonarQube analysis') {
      steps {
        script {
          scannerHome = tool 'SonarQubeScanner'
        }
        withSonarQubeEnv('SonarQube') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
    // At least one stage is required.
    stage("Integration test") {
      // Every stage must have a steps block containing at least one step.
      steps {
            sh "docker-compose up -d"
            sh "docker-compose exec -T apache-service  docker/install/./prepare.sh"
        }
      // Post can be used both on individual stages and for the entire build.
      post {
        failure {
            mail body: "The following project build failed: ${env.BUILD_URL} ,  Node: ${env.NODE_NAME} , Project workspace: ${env.WORKSPACE} , Repository: ${env.GIT_URL}" ,
            from: 'jenkinssquare@gmail.com',
            replyTo: 'jenkinssquare@gmail.com',
            subject: 'project build failed',
            to: 'ldelahaye@square.nl'
        }
      }
}
    // Att least one stage is required.
    stage("Unit testing") {
      // Every stage must have a steps block containing at least oned step.
      steps {
        sh "docker-compose exec -T apache-service  vendor/bin/phpunit -c tests/phpunit.xml"
        }
      // Post can be used both on individual stages and for the entire build.
      post {
        failure {
            mail body: "The following project build failed: ${env.BUILD_URL} ,  Node: ${env.NODE_NAME} , Project workspace: ${env.WORKSPACE} , Repository: ${env.GIT_URL}" ,
            from: 'jenkinssquare@gmail.com',
            replyTo: 'jenkinssquare@gmail.com',
            subject: 'project build failed',
            to: 'ldelahaye@square.nl'
        }
      }
  }
}
  post {
    //Always runs. And it runs before any of the other post conditions.
  //  always {
  //       //  sh "docker-compose down"
  // }
    success {
            mail body: "The following project build succeeded: ${env.BUILD_URL} ,  Node: ${env.NODE_NAME} , Project workspace: ${env.WORKSPACE} , Repository: ${env.GIT_URL}" ,
            from: 'jenkinssquare@gmail.com',
            replyTo: 'jenkinssquare@gmail.com',
            subject: 'project build succeeded!!',
            to: 'ldelahaye@square.nl'
    }
    
    failure {
            mail body: "The following project build failed: ${env.BUILD_URL} ,  Node: ${env.NODE_NAME} , Project workspace: ${env.WORKSPACE} , Repository: ${env.GIT_URL}" ,
            from: 'jenkinssquare@gmail.com',
            replyTo: 'jenkinssquare@gmail.com',
            subject: 'project build failed',
            to: 'ldelahaye@square.nl'
    }
  }
}
