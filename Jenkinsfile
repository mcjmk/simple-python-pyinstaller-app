pipeline {
    agent {
        docker {
            image 'cdrx/pyinstaller-windows:python3'
            args '-u root --privileged'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'wine pyinstaller --onefile sources/add2vals.py'
                }
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals.exe'
                }
            }
        }
    }
}