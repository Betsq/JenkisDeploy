//def ReleaseDir = "c:\inetpub\wwwroot"
pipeline {
			agent any
			stages {
				stage('Source'){
					steps{
						checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Betsq/JenkisDeploy']]])
					}
				}
				stage('StopIISApp'){
					steps{
						bat "powershell.exe Stop-Website -Name 'Default Web Site'"
					}
				}
				stage('Build') {
    					steps {
							sleep 5
    					    bat "\"${tool 'MSBuild'}\" JenkisDeploy.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=True /p:publishUrl=c:\\inetpub\\wwwroot"
    					}
				}
			}
}


//environment {
//   PROJ_NAME = 'JenkisDeploy'
//}

//def dotnet_build(){
//	dir("${env.PROJ_NAME}") {
//		sh(script: "dotnet build ${env.PROJ_NAME}.csproj", returnStdout: true);
//    }
//}