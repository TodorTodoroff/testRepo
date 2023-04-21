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
                script 
                {
                    def buildContentFile = readFile "${env.WORKSPACE}/file.xml"
                    echo "${buildContentFile}"

                    def parsedXml = new XmlSlurper().parseText(buildContentFile)

                    def node16version = parsedXml.runtimes.node_16.version[0].toString()
                    echo "${node16version}"

                    def repoVersion = "https://nodejs.org/dist/latest-v16.x/".toURL().text
                    println repoVersion
                    def filteredVersion = (repoVersion =~ /[0-9]{2}\.[0-9]{2}\.[0-9]*/)[0]
                    println filteredVersion
                    println filteredVersion.equalsIgnoreCase(node16version)

                    if(filteredVersion.equalsIgnoreCase(node16version)) {
                        currentBuild.result = 'SUCCESS'
                        return
                    } else {
                        currentBuild.result = 'FAILURE'
                        return
                    }
                }
            }
        }
        stage('Something Else')
        {
            steps
            {
                script
                {
                    println 'HELLO VIETNAM'
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
