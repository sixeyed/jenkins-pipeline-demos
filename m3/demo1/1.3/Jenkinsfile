pipeline {
    agent any
    parameters {
        booleanParam(name: 'RC', defaultValue: false, description: 'Is this a Release Candidate?')
    }
    environment {
        VERSION = "0.1.0"        
        VERSION_RC = "rc.2"
    }
    stages {
        stage('Audit tools') {                        
            steps {
                auditTools()
            }
        }
        stage('Build') {
            environment {
                VERSION_SUFFIX = getVersionSuffix()
            }
            steps {
              echo "Building version: ${VERSION} with suffix: ${VERSION_SUFFIX}"
              sh 'dotnet build -p:VersionPrefix="${VERSION}" --version-suffix "${VERSION_SUFFIX}" ./m3/src/Pi.Web/Pi.Web.csproj'
            }
        }
        stage('Unit Test') {
            steps {
              dir('./m3/src') {
                sh '''
                    dotnet test --logger "trx;LogFileName=Pi.Math.trx" Pi.Math.Tests/Pi.Math.Tests.csproj
                    dotnet test --logger "trx;LogFileName=Pi.Runtime.trx" Pi.Runtime.Tests/Pi.Runtime.Tests.csproj
                '''
                mstest testResultsFile:"**/*.trx", keepLongStdio: true
              }
            }
        }
        stage('Smoke Test') {
            steps {
              sh 'dotnet ./m3/src/Pi.Web/bin/Debug/netcoreapp3.1/Pi.Web.dll'
            }
        }
        stage('Publish') {
            when {
                expression { return params.RC }
            } 
            steps {
                sh 'dotnet publish -p:VersionPrefix="${VERSION}" --version-suffix "${VERSION_RC}" ./m3/src/Pi.Web/Pi.Web.csproj -o ./out'
                archiveArtifacts('out/')
            }
        }
    }
}

String getVersionSuffix() {
    if (params.RC) {
        return env.VERSION_RC
    } else {
        return env.VERSION_RC + '+ci.' + env.BUILD_NUMBER
    }
}

void auditTools() {
    sh '''
        git version
        docker version
        dotnet --list-sdks
        dotnet --list-runtimes
    '''
}