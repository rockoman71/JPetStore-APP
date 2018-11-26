node { 
    def gitRepo = "https://github.com/rockoman71/JPetStore-APP.git"

    stage('Get source') {
		   git gitRepo
	  }
   
    stage('Build APP') {
 
	    withAnt(installation: 'Ant 1.10.5') {
               sh "ant all"
            }
    } 
   

  stage('UrbanCode create version') {
   // class: Defines the Pipeline plug-in to define. The UCD Jenkins Pipeline plug-in’s class name is UCDeployPublisher
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd',
	component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
           componentName: 'JPetStore-APP',
           delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '${BUILD_NUMBER}',
		baseDir: '/var/lib/jenkins/workspace/JPetStore-APP/dist/',
                fileIncludePatterns: 'JPetStore.war',
                fileExcludePatterns: '',                
                pushDescription: 'Pushed from Jenkins'
	    ]	
	]	
    ])
  }
	
stage('UrbanCode Deploy to Dev') {	
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd',
        deploy: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
                deployApp: 'JPetStore',
                deployEnv: 'DEV-1',
                deployProc: 'Deploy JPetStore',            
                deployVersions: 'JPetStore-APP:${BUILD_NUMBER}\nJPetStore-WEB:latest',		
                deployOnlyChanged: true
        ]		
    ])
  }
}
