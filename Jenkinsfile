pipeline
{
    agent
    {
        label 'docker'
    }
    environment
    {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub')
    }
    stages
    {
        stage('Clone')
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/TodorTodoroff/testRepo.git'
            }
        }
        stage('Execute the version checks')
        {
            steps
            {
                script {
                    def File = readFile "${env.WORKSPACE}/file.xml"
                    echo "${File}"

                    def xml = new XmlSlurper().parseText(File)

                    def env = xml.runtimes.node_16.version[0]
                    echo "${env}"
                    def envtest = env.toString()
                    echo "${envtest}"

                    def test = "https://nodejs.org/dist/latest-v16.x/".toURL().text
                    println test
                    def result = (test =~ /[0-9]{2}\.[0-9]{2}\.[0-9]*/)[0]
                    println result
                    println result.equalsIgnoreCase(envtest)

                    if(result.equalsIgnoreCase(envtest)) {
                        println "YEAH"
                        currentBuild.result = 'SUCCESS'
                        return
                    } else {
                        println "NAH"
                        currentBuild.result = 'FAILURE'
                        return
                    }
                }
            }
        }
    }
    post
    {
        always
        {
            cleanWs()
        }
    }
}
