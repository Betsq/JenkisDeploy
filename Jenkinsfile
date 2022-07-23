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
				stage("Delete files"){
					steps{
						powershell "Remove-Item c:\\inetpub\\wwwroot\\* -Exclude web.config -Recurse -Force";
					}
				}
				stage('Build') {
    					steps {
    					    bat "\"${tool 'MSBuild'}\" JenkisDeploy.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=False /p:publishUrl=c:\\inetpub\\wwwroot"
    					}
				}
				stage('Replace web config'){
					steps{
						powershell "Clear-Content 'c:\\inetpub\\wwwroot\\web.config'"
						powershell "Add-Content 'c:\\inetpub\\wwwroot\\web.config' 'End of file'"
					}
				}
				
			}
}	




//def dotnet_build(){
//	dir("${env.PROJ_NAME}") {
//		sh(script: "dotnet build ${env.PROJ_NAME}.csproj", returnStdout: true);
//    }
//}