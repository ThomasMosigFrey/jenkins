#!/usr/bin/env groovy
node ("windows") {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://thomasfrey@bitbucket.org/thomasfrey/jenkins_repository.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'MAVEN'
   }
   stage('Build') {
      dir('SimpleApp') {
         // Run the maven build
         if (isUnix()) {
            sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
         } else {
            bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      dir('SimpleApp') {
         archive 'target/*.jar'
      }
   }
}