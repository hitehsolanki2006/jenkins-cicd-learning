pipeline {
    agent any

    environment {
        EC2_IP     = "15.206.148.105"
        SSH_USER   = "ubuntu"
        PPK_KEY    = "D:\\AWS\\KeyFiles\\my-keypair.ppk"
        LOCAL_CODE = "D:\\Learn_DevOps_with_Testing\\WebTest\\aws"
        PSCP       = "C:\\Program Files\\PuTTY\\pscp.exe"
        PLINK      = "C:\\Program Files\\PuTTY\\plink.exe"

        HOST_KEY = "ssh-ed25519 255 SHA256:QyyThVd7oXGCiKBrjz01JYxvz3IScIFQ0licAxzvbxg"
    }

    stages {

        stage('Deploy via PSCP') {
            steps {
                bat """
                echo Deploying site using PSCP...

                "%PSCP%" ^
                  -batch ^
                  -hostkey "%HOST_KEY%" ^
                  -i "%PPK_KEY%" ^
                  -r "%LOCAL_CODE%\\*" ^
                  %SSH_USER%@%EC2_IP%:/var/www/testsite
                """
            }
        }

        stage('Reload Nginx') {
            steps {
                bat """
                "%PLINK%" ^
                  -batch ^
                  -hostkey "%HOST_KEY%" ^
                  -i "%PPK_KEY%" ^
                  %SSH_USER%@%EC2_IP% ^
                  "sudo systemctl reload nginx"
                """
            }
        }
    }
}
