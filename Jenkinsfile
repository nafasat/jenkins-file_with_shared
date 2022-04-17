@Library('jenkins-demo') _
def credential_sftp_name = 'sftp-local-username-pwd'
def credential_github_name = 'github-ssh-connect'
pipeline {
    agent any
    parameters
    {
        string defaultValue: '', description: 'Please enter the artifact location in SFTP under admin dir', name: 'sftp_path'
        string defaultValue: 'ansible.zip', description: 'Please enter the tar archive file name', name: 'tar_archive_name'
        string defaultValue: '', description: 'Please enter the commit message', name: 'commit_msg'
        string defaultValue: '.', description: 'Please enter the target directory in the repo wit', name: 'target_dir'
        string defaultValue: 'localhost', description: 'You may change SFTP_IP', name: 'sftp_ip'
        string defaultValue: 'github.com/nafasat/testing_git.git', description: 'You may change SFTP_IP', name: 'repo_name_without_https'
        string defaultValue: 'master', description: 'You may git clone branch name', name: 'pull_from_branch'        
        string defaultValue: '', description: 'You may change git push feature branch name', name: 'push_to_feature_branch'
    }    
    stages {
        stage ('Push_to_GitHub') {
            steps {
                script {
                        push_to_github.push_github_auth_based(credential_github_name: credential_github_name, credential_sftp_name: credential_sftp_name, zip_file_name:"${tar_archive_name}", sftp_ip:"${sftp_ip}", repo_name_without_https: "${repo_name_without_https}", commit_msg: "${commit_msg}", pull_from_branch_name: "${pull_from_branch}")
                }
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs deleteDirs: true, patterns: [[pattern: '*', type: 'INCLUDE']]
        }
    }
}
