node {
    stage('Build') {
        docker.image('python:2-alpine').inside('-v $WORKSPACE:/workspace') {
            sh 'ls -l /workspace/sources/'
            sh 'python -m py_compile /workspace/sources/add2vals.py /workspace/sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }

    stage('Deliver') {
        docker.image('cdrx/pyinstaller-linux:python2').inside {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        archiveArtifacts artifacts: 'dist/add2vals'
    }
}
