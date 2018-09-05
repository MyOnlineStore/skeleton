pipeline {
  agent any
  stages {
    stage('Dependencies') {
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      parallel {
        stage('Composer') {
          agent {
            dockerfile {
              dir 'docker/php'
              args '-v ${COMPOSER_CACHE_DIR}:${COMPOSER_CACHE_DIR}'
              reuseNode true
            }
          }
          steps  {
            sh 'php bin/composer install --prefer-dist --optimize-autoloader --no-progress --no-interaction'
          }
        }
        stage('Docker Network') {
          steps {
            sh "docker network create ${BUILD_TAG}"
          }
        }
      }
    }
    stage('Cache') {
      agent {
        dockerfile {
          dir 'docker/php'
          reuseNode true
        }
      }
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      steps {
        sh "true"
      }
    }
    stage('Test: Unit') {
      agent {
        dockerfile {
          dir 'docker/php'
          reuseNode true
        }
      }
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      steps {
        sh "true"
      }
    }
    stage('Test: Integration') {
      agent {
        dockerfile {
          dir 'docker/php'
          args "--network ${BUILD_TAG} --network-alias php --network-alias php-xdebug"
          reuseNode true
        }
      }
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      steps {
        sh "true"
      }
    }
    stage('Test: Functional') {
      agent {
        dockerfile {
          dir 'docker/php'
          args "--network ${BUILD_TAG} --network-alias php --network-alias php-xdebug"
          reuseNode true
        }
      }
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      steps {
        sh "true"
      }
    }
    stage('Test: Acceptance') {
      agent {
        dockerfile {
          dir 'docker/php'
          args "--network ${BUILD_TAG} --network-alias php --network-alias php-xdebug"
          reuseNode true
        }
      }
      when {
        beforeAgent true
        anyOf {
          branch 'master'
          branch 'release'
          expression { return env.CHANGE_ID != null }
        }
      }
      steps {
        sh "true"
      }
    }
    stage('Deployment') {
      steps {
        sh "true"
      }
    }
  }
  post {
    always {
      sh "docker network rm ${BUILD_TAG} || true"
    }
  }
  environment {
    COMPOSER_CACHE_DIR = '/var/cache/composer'
  }
}
