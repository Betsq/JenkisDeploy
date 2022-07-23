//def ReleaseDir = "c:\inetpub\wwwroot"
environment {
   PROJ_NAME = 'JenkisDeploy'
   DESTINATION_FOLDER = 'C:\\inetpub\\wwwroot';
}

pipeline {
	agent any
	stages {
		stage('Source'){
			steps{
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Betsq/JenkisDeploy']]])
			}
		}
		stage('Stop the application in IIS'){
			steps{
				powershell "Stop-Website -Name 'Default Web Site'"
			}
		}
		
		stage('Build') {
				steps {
					bat "\"${tool 'MSBuild'}\" JenkisDeploy.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=True /p:publishUrl=${env.DESTINATION_FOLDER}"
				}
		}
		stage('Start the application in IIS'){
			steps{
				powershell "Start-Website -Name 'Default Web Site'"
			}
		}
	}
}	




//def dotnet_build(){
//	dir("${env.PROJ_NAME}") {
//		sh(script: "dotnet build ${env.PROJ_NAME}.csproj", returnStdout: true);
//    }
//}