@Library('jenkins-demo') _
def credential_sftp_name = 'sftp-local-username-pwd'
def credential_github_name = 'github-ssh-connect'
pipeline {
    agent any
    parameters
    {
        string defaultValue: '/SMEUAT/CLO', description: 'Please enter the artifact location in SFTP under admin dir', name: 'sftp_path'
        string defaultValue: 'ansible.zip', description: 'Please enter the tar archive file name', name: 'tar_archive_name'
        string defaultValue: '', description: 'Please enter the commit message', name: 'commit_msg'
        string defaultValue: '', description: 'Please enter the target directory in the repo', name: 'target_dir'
        string defaultValue: 'localhost', description: 'You may change SFTP_IP', name: 'sftp_ip'
    }    
    stages {
        stage('Get_SFTP') {
            steps {
                script {
                    jenkins_cd.sftp_get(credential_sftp_name: credential_sftp_name,sftp_ip: "${sftp_ip}", tar_archive_name:"${tar_archive_name}", target_path:"${target_dir}")
                }
            }
        }
        stage ('Push_to_GitHub') {
            steps {
                script {
                        jenkins_cd.push_github(credential_github_name: credential_github_name, archive_name:"${tar_archive_name}", target: "${target_dir}", commit_msg: "${commit_msg}", jenkins_user_email:'nefs.cs@gmail.com', jenkins_user: 'naafsat')
                }
            }
        }
        stage('execute_shell_script') {
            steps {
                script {
                    helloworldexternal(first:"Nafasat",second:"Ahmed")
                }
            }
        }
    }
}
