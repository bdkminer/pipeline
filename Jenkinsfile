pipeline {
  agent {

    docker {
      image '158.160.133.56:8123/jenkins-agent:1.0.1'
      args '-v /var/run/docker.sock:/var/run/docker.sock -v /etc/group:/etc/group:ro'
    }

  }

  stages {

    stage('Copy source with configs') {
      steps {
        sh 'mkdir /app && cd /app'
        git 'https://github.com/daticahealth/java-tomcat-maven-example.git'
      }
    }

    stage('Build war') {
      steps {
        sh 'cd /app/java-tomcat-maven-example'
        sh 'mvn package'
      }
    }

    stage('Make docker image') {
      steps {
        sh 'cd /app'
        git 'https://github.com/bdkminer/pipeline.git'
        sh 'cp /app/pipeline/Dockerfile /app && rm -r app/pipeline && docker build --tag=mywebapp:1.0.0 .'
        sh 'docker push 158.160.133.56:8123/mywebapp:1.0.0'

      }
    }

    stage('Run docker on prod') {
      steps {
        sh '''ssh root@158.160.153.35 << EOF
	sudo docker pull 158.160.133.56:8123/mywebapp:1.0.0
	sudo docker run -d -p 8080:8080 158.160.133.56:8123/mywebapp:1.0.0
EOF'''
      }
    }
  }
}