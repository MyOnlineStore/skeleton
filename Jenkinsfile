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
            docker {
              image 'composer'
              args '-v ${COMPOSER_CACHE_DIR}:${COMPOSER_CACHE_DIR}'
              reuseNode true
            }
          }
          steps  {
            sh 'composer install --prefer-dist --optimize-autoloader --no-progress --no-interaction'
          }
        }
        stage('Docker Network') {
          steps {
            sh "docker network create ${BUILD_TAG}"
          }
        }
      }
      environment {
        COMPOSER_CACHE_DIR = '/var/cache/composer'
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
          dir 'docker/php-xdebug'
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
        sh 'vendor/bin/phpunit --coverage-clover=var/ci/clover.xml --coverage-text=var/ci/coverage.txt --coverage-html=var/ci'
        script {
          COVERAGE_PERCENTAGE = sh (
            script: "awk -F':' '/^  Lines:/{print \$2}' var/ci/coverage.txt | awk {'print \$1'}",
            returnStdout: true
          ).trim()

          if (COVERAGE_PERCENTAGE) {
            step([$class: 'GitHubCommitStatusSetter', contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: "continuous-integration/jenkins/coverage"], statusResultSource: [$class: 'ConditionalStatusResultSource', results: [[$class: 'AnyBuildResult', message: "${COVERAGE_PERCENTAGE}", state: 'SUCCESS']]]])
          }
        }
      }
      post {
        always {
          step([
            $class: 'CloverPublisher',
            cloverReportDir: 'var/ci',
            cloverReportFileName: 'clover.xml',
            healthyTarget: [
              conditionalCoverage: 80,
              methodCoverage: 70,
              statementCoverage: 80
            ],
            unhealthyTarget: [
              methodCoverage: 50,
              conditionalCoverage: 50,
              statementCoverage: 50
            ],
            failingTarget: [
              methodCoverage: 0,
              conditionalCoverage: 0,
              statementCoverage: 0
            ]
          ])
        }
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
}
