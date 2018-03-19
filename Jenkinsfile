node {
  def mvnHome
  stage('Preparation') { // for display purposes
    // Get some code from a GitHub repository
    git credentialsId: 'ec8087f2-8cc0-4d65-99bc-5efdd0085d6f', url: 'https://github.com/niklaushirt/libertydemo.git'
    // Get the Maven tool.
    // ** NOTE: This 'M3' Maven tool must be configured
    // **       in the global configuration.
    mvnHome = tool 'M3'
  }
  stage('CI-Build') {
    // Run the maven build
    if (isUnix()) {
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore package"
    } else {
      bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore package/)
    }
  }
  stage('CI-Docker-Build') {
    sh "cd '/home/demo/jenkins/workspace/$JOB_BASE_NAME/target/'"
    sh "ls"
    docker.withTool("docker") {
    def customImage = docker.build("liberty_demo:demo")
    //customImage.push('demo')
    }

  }
  stage('CI-Push-To-UrbanCode') {
    step([$class: 'UCDeployPublisher',
      siteName: 'UCD',
      component: [
        $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
        componentName: 'LIBERTY',
        delivery: [
          $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
          pushVersion: '${BUILD_NUMBER}',
          baseDir: '/home/demo/jenkins/workspace/$JOB_BASE_NAME/',
          fileIncludePatterns: '*.*',
          fileExcludePatterns: '',
          //pushProperties: 'jenkins.server=Local\njenkins.reviewed=false',
          pushDescription: 'Pushed from Jenkins'
        ]
      ]
    ])
  }
  stage('CD-Deploy-To-ICP') {
   step([$class: 'UCDeployPublisher',
        siteName: 'UCD',
        deploy: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
            deployApp: 'DEMO',
            deployEnv: 'TEST',
            deployProc: 'deploy',
            deployVersions: 'LIBERTY:${BUILD_NUMBER}',
            //deployVersions: 'LIBERTY:49',
            createSnapshot: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$CreateSnapshotBlock',
                snapshotName: 'LIBERTY_SNAPSHOT_${BUILD_NUMBER}'
            ],
            deployOnlyChanged: false
        ]
       
    ])
}
}
