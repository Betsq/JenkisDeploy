//def ReleaseDir = "c:\inetpub\wwwroot"
environment {
   PROJ_NAME = 'JenkisDeploy'
   DESTINATION_FOLDER = 'C:\\inetpub\\wwwroot';
}

pipeline {
	agent any
	environment {
   		PROJ_NAME = 'JenkisDeploy';
   		DESTINATION_FOLDER = 'C:\\inetpub\\wwwroot';
		APP_NAME_IN_IIS = 'Default Web Site';
		WEB_CONFIG_PATH = 'C:\\Users\\betsq\\Desktop\\web.config';
	}

	stages {
		stage('Source'){
			steps{
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Betsq/JenkisDeploy']]])
			}
		}
		stage('Stop the application in IIS'){
			steps{
				powershell "Stop-Website -Name '${env.APP_NAME_IN_IIS}'"
			}
		}
		stage("Delete files"){
			steps{
				powershell "Remove-Item ${env.DESTINATION_FOLDER}\\* -Exclude web.config -Recurse -Force";
			}
		}
		stage('Restore packages'){
			steps{
				bat "dotnet restore JenkisDeploy\\JenkisDeploy.csproj"
			}
		}
		stage('Clean'){
			steps{
				bat "dotnet clean JenkisDeploy\\JenkisDeploy.csproj"
			}
		}
		stage('Build') {
				steps {
					bat "\"${tool 'MSBuild'}\" JenkisDeploy.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=False /p:publishUrl=${env.DESTINATION_FOLDER}"
				}
		}
		stage('Replace web config'){
			steps{
				powershell "Copy-Item '${env.WEB_CONFIG_PATH}' -Destination '${env.DESTINATION_FOLDER}'"
			}
		}
		stage('Start the application in IIS'){
			steps{
				powershell "Start-Website -Name '${env.APP_NAME_IN_IIS}'"
			}
		}
	}
}	




//def dotnet_build(){
//	dir("${env.PROJ_NAME}") {
//		sh(script: "dotnet build ${env.PROJ_NAME}.csproj", returnStdout: true);
//    }
//}