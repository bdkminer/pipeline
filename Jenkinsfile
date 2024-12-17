pipeline {
  agent {

    docker {
      image 'devcvs-srv01:5000/shop2-backend/jenkins-agent'
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
        sh 'cp -R gateway-api/build/libs/* docker-setup/shop/gateway-api && cd docker-setup/shop/gateway-api && docker build --tag=gateway-api .'
        sh 'docker push devcvs-srv01:5000/shop2-backend/gateway-api:2-staging'

      }
    }

    stage('Run docker on prod') {
      steps {
        sh '''ssh root@51.250.44.229 << EOF
	sudo docker pull devcvs-srv01:5000/shop2-backend/gateway-api:2-staging
	cd /etc/shop/docker
	sudo docker-compose up -d
EOF'''
      }
    }
  }