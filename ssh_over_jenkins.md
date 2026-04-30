```bash
pipeline {
    agent any
    stages {
        stage("scp") {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: '<server-name>', //Change the name of the server
                        transfers: [
                            sshTransfer(
                                sourceFiles: '<filename>', //Change
                                remoteDirectory: '<destination_path>',//Change
                                execCommand: ''    // leave empty, no command needed
                            )
                        ]
                    )
                ])
            }
        }
    }
}
```
##Steps:
1. Go to manage jenkin, 
2. install a plugin named "publish over ssh"
3. Go to system setting, and add ssh server name, host, and username and pemfile
4. test connetion, if connection is succesfull, 
5. copy script and run
