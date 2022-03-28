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
                    jenkins_cd.sftp_get(credential_sftp_name: credential_sftp_name,sftp_ip: "${sftp_ip}", tar_archive_name:"${tar_archive_name}", commit_msg:"${commit_msg}")
                }
            }
        }
        stage ('Push_to_GitHub') {
            steps {
                script {
                    //jenkins_cd.push_github(credential_github_name: credential_github_name,tar_archive_name:"${tar_archive_name}",target: "${target_dir}")
                    withCredentials([usernamePassword(credentialsId: "github-ssh-connect", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                      dir("${WORKSPACE}/${target_dir}")
                      {
                        sh ''' git clone https://github.com/nafasat/testing_git.git
                          unzip "${tar_archive_name}"
                          rm -rf "${tar_archive_name}"
                          cp ./ansible.cfg /testing_git/
                          cd /testing_git/
                          git checkout dtesting
                          git add -A
                          git commit -m "${commit_msg}"
                          git push https://"${USERNAME}:${PASSWORD}@github.com/nafasat/testing_git.git'''
                      }
                    }
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
