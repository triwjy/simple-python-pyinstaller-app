node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash includes: 'sources/*.py*', name: 'compiled-results'
        }
    }

    stage('Test') {
        try {
            docker.image('qnib/pytest').inside{
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } catch (err) {
            echo "Caught: ${err}"
            currentBuild.result = 'FAILURE'
        } finally {
            junit 'test-reports/results.xml'
        }
    }
}