#!groovy
pipeline {
 agent any
  stages {
    stage('Prepare') {
      steps {
        sh "mkdir -p log"
        sh "touch log/prepare.log.txt"
        sh "git submodule update --init > log/prepare.log.txt 2>&1"
	sh "wget https://sourceforge.net/projects/genode/files/genode-toolchain/16.05/genode-toolchain-16.05-x86_64.tar.bz2 >> log/prepare.log.txt 2>&1"
	sh "tar xfj genode-toolchain-16.05-x86_64.tar.bz2 -C . >> log/prepare.log.txt 2>&1"
	sh "touch genode/build/focnados_pbxa9/etc/tools.conf >> log/prepare.log.txt 2>&1"
	sh "echo CROSS_DEV_PREFIX=\$(pwd)/usr/local/genode-gcc/bin/genode-arm- >> genode/build/focnados_pbxa9/etc/tools.conf"
        sh "make ports >> log/prepare.log.txt 2>&1"
      }
    }
    stage('Build') {
      steps {
        sh 'make jenkins_build_dir'
        sh 'make jenkins_run'
      }
    }
    stage('Notifications') {
      steps {
       sh "echo nofitications stage"
      }
    }
  } // stages ends here
  post {
   failure {
    mattermostSend color: "#E01818", message: "Build Failed: [${env.JOB_NAME} ${env.BUILD_NUMBER}](${env.BUILD_URL}) ; See more log information here: https://nextcloud.os.in.tum.de/apps/files/?dir=/702nados/log/${env.JOB_NAME}/${env.BUILD_NUMBER}/"
   }
   success {
    mattermostSend color: "#3cc435", message: "Build Successful: [${env.JOB_NAME} ${env.BUILD_NUMBER}](${env.BUILD_URL}) ; See more log information here: https://nextcloud.os.in.tum.de/apps/files/?dir=/702nados/log/${env.JOB_NAME}/${env.BUILD_NUMBER}/"
   }
   always {
      sh "mkdir -p /home/bliening/ownCloud/702nados/log/${env.JOB_NAME}/${env.BUILD_NUMBER}"
      sh "cp -R log/* /home/bliening/ownCloud/702nados/log/${env.JOB_NAME}/${env.BUILD_NUMBER}/"
     deleteDir()
   }
  } // post ends here
} // pipeline ends here
