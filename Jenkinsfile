pipeline {
    agent any
    stages {
        stage('Obtener_Codigo'){
            steps{
                git url: 'https://github.com/jenkinsci/git-tag-message-plugin'
                GIT_TAG_NAME = gitTagName()
                GIT_TAG_MESSAGE = gitTagMessage()
            }
        }
    }
}

String gitTagName() {
    commit = getCommit()
    if (commit) {
        desc = sh(script: "git describe --tags ${commit}", returnStdout: true)?.trim()
        if (isTag(desc)) {
            return desc
        }
    }
    return null
}
 
/** @return The tag message, or `null` if the current commit isn't a tag. */
String gitTagMessage() {
    name = gitTagName()
    msg = sh(script: "git tag -n10000 -l ${name}", returnStdout: true)?.trim()
    if (msg) {
        return msg.substring(name.size()+1, msg.size())
    }
    return null
}
 
String getCommit() {
    return sh(script: 'git rev-parse HEAD', returnStdout: true)?.trim()
}