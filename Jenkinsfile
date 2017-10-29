node {
   stage ('Checkout') {
			 def scmVars = checkout scm
       //def scmVars = checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: '/H/Programming/Workspaces/jenkins']]])

			 def commitHash = scmVars.GIT_COMMIT.take(7)
			 echo "Git ref: $commitHash"
    }

    try {
        stage ('Build') {
            withMaven(maven: 'maven-3.5.0') {
							bat "mvn -B -f mvn-test-project/pom.xml versions:set -DnewVersion=${version}"
              bat "mvn -B -f mvn-test-project/pom.xml clean package -DskipTests"
            }
        }
        stage ('Test') {
            withMaven(maven: 'maven-3.5.0') {
                bat "mvn -B -f mvn-test-project/pom.xml verify"
            }
        }
    } finally {
        emailextrecipients([[$class: 'CulpritsRecipientProvider']])
    }
}
