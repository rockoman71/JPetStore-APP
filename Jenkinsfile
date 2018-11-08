node { 
    def gitRepo = "https://github.com/rockoman71/JPetStore-APP.git"

    stage('Get source') {
		   git gitRepo
	  }
   
    stage('Build APP') {
 
	    withAnt(installation: 'Ant 1.10.5') {
               sh "ant default"
            }
    } 
   

  stage('UrbanCode create version') {
   // class: Defines the Pipeline plug-in to define. The UCD Jenkins Pipeline plug-in’s class name is UCDeployPublisher
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd',
	component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
           componentName: 'Blue-Green Demo-bgdemo',
           delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '${BUILD_NUMBER}',
		baseDir: '.',
                fileIncludePatterns: 'README.md',
                fileExcludePatterns: '',
                pushProperties: 'dockerImageTag=${BUILD_NUMBER}\ndockerImageId=x',
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
                deployApp: 'Blue-Green Demo',
                deployEnv: 'Dev',
                deployProc: 'Deploy Application from YAML',            
                deployVersions: 'Blue-Green Demo-bgdemo:${BUILD_NUMBER}\nbgdemo Webpage:2.2',
		//deployVersions: 'Blue-Green Demo-bgdemo:${BUILD_NUMBER}\nbgdemo Webpage:latest',
                deployOnlyChanged: false
        ]		
    ])
  }
}
