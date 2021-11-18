def getBranch() {
    def patch_name = sh(script: "basename \$(dirname ${env.WORKSPACE})", returnStdout: true).trim()
    return patch_name;
}

def environment_url = "https://git.netcracker.com/SD.DP.Deployment/Rakuten/Ansible-Inventory.git"
def installer_subfolder = "Install"

pipeline{
    agent any    
    options {
      timestamps()
      ansiColor('xterm')
      buildDiscarder(logRotator(numToKeepStr: '15'))
    }
    parameters {
      booleanParam(name: 'RBM', defaultValue: false, description: '')
      text(name: 'rbm_patches', defaultValue: "", description: 'RBM Patches URLs.')
      booleanParam(name: 'AMM', defaultValue: false, description: '')
      text(name: 'amm_patches', defaultValue: "", description: 'AMM Patches URLs.')
      booleanParam(name: 'DOC1_EngageOne', defaultValue: false, description: '')
      text(name: 'doc1_engageone_patches', defaultValue: "", description: 'EngageOne Patches URLs.')
      booleanParam(name: 'TOMS', defaultValue: false, description: '')
      text(name: 'toms_patches', defaultValue: "", description: 'TOMS Patches URLs.')
      booleanParam(name: 'Active_Broker', defaultValue: false, description: '')
      text(name: 'ab_patches', defaultValue: "", description: 'Active_Broker Patches URLs.')
      booleanParam(name: 'Mobile_Gateway', defaultValue: false, description: '')
      text(name: 'mg_patches', defaultValue: "", description: 'Mobile_Gateway Patches URLs.')
      booleanParam(name: 'API_Gateway', defaultValue: false, description: '')
      text(name: 'api_patches', defaultValue: "", description: 'API_Gateway Patches URLs.')
      booleanParam(name: 'SSP_eCommerce_eCare', defaultValue: false, description: '')
      text(name: 'ssp_ecare_patches', defaultValue: "", description: 'SSP eCare eCommerce Patches URLs.')
      booleanParam(name: 'SSP_eCSR', defaultValue: false, description: '')
      text(name: 'ssp_ecsr_patches', defaultValue: "", description: 'SSP SSP CSR POS Patches URLs.')
      booleanParam(name: 'SSP_eCPOS', defaultValue: false, description: '')
      text(name: 'ssp_ecpos_patches', defaultValue: "", description: 'SSP Customer POS Patches URLs.')
      booleanParam(name: 'TBAPI', defaultValue: false, description: '')
      text(name: 'tbapi_patches', defaultValue: "", description: 'TBAPI Patches URLs.')
      string(name: 'installer_link', defaultValue: "", description: '')
    }
    stages{
        stage('Clone repo'){
            steps{
                dir("$WORKSPACE"){
                    deleteDir()
                }
                git branch: getBranch(),
                    credentialsId: "configuration",
                    url: "${environment_url}"
                script {
                    currentBuild.displayName = "#${currentBuild.number}: "
                }
            }
        }
        stage('Parallel') {
            parallel {
                stage('RBM & DOC1') {
                    stages {
                        stage('RBM') {
                            when { 
                                expression { return params.RBM } 
                            }
                            steps {
                                echo "Running RBM installation: ${rbm_patches}"
                                build job: "${installer_subfolder}/Install_RBM", 
                                parameters: [
                                    string(name: 'configuration_git', value: environment_url),
                                    string(name: 'configuration_branch', value: getBranch()),
                                    string(name: 'configuration_id', value: "configuration"),
                                    string(name: 'patches', value: rbm_patches.replaceAll('172.23.253.40','rkt2040')),
                                    string(name: 'INSTALLER', value: "${installer_link}"),
                                    string(name: 'ssh_id', value: "rbm_ssh")
                                ]
                            }
                        }
                        stage('DOC1_EngageOne') {
                            when { 
                                expression { return params.DOC1_EngageOne } 
                            }
                            steps {
                                echo "Running DOC1_EngageOne installation: ${doc1_engageone_patches}"
                                build job: "${installer_subfolder}/Install_EngageOne", 
                                parameters: [
                                    string(name: 'configuration_git', value: environment_url),
                                    string(name: 'configuration_branch', value: getBranch()),
                                    string(name: 'configuration_id', value: "configuration"),
                                    string(name: 'patches', value: doc1_engageone_patches.replaceAll('172.23.253.40','rkt2040')),
                                    string(name: 'autoinstaller_patch_options', value: "application.server.platform=auto"),
                                    string(name: 'INSTALLER', value: "${installer_link}"),
                                    string(name: 'ssh_id', value: "rbm_ssh")
                                ]
                            }
                        }
                    }
                }
                stage('AMM') {
                    when { 
                        expression { return params.AMM } 
                    }
                    steps {
                        echo "Running AMM installation: ${amm_patches}"
                        build job: "${installer_subfolder}/Install_AMM", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: amm_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "rbm_ssh")
                        ]
                    }
                }
                stage('TOMS') {
                    when { 
                        expression { return params.TOMS } 
                    }
                    steps {
                        echo "Running TOMS installation: ${toms_patches}"
                        build job: "${installer_subfolder}/Install_TOMS", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: toms_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('Active_Broker') {
                    when { 
                        expression { return params.Active_Broker } 
                    }
                    steps {
                        echo "Running Active Broker installation: ${ab_patches}"
                        build job: "${installer_subfolder}/Install_Active_Broker", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: ab_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('Mobile_Gateway') {
                    when { 
                        expression { return params.Mobile_Gateway } 
                    }
                    steps {
                        echo "Running Mobile Gateway installation: ${mg_patches}"
                        build job: "${installer_subfolder}/Install_Mobile_Gateway", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: mg_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('API_Gateway') {
                    when { 
                        expression { return params.API_Gateway } 
                    }
                    steps {
                        echo "Running API Gateway installation: ${api_patches}"
                        build job: "${installer_subfolder}/Install_API_Gateway", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: api_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('SSP eCommerce eCare') {
                    when { 
                        expression { return params.SSP_eCommerce_eCare } 
                    }
                    steps {
                        echo "Running SSP eCommerce eCare installation: ${ssp_ecare_patches}"
                        build job: "${installer_subfolder}/Install_SSP", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: ssp_ecare_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'ansible_extra_args', value: "-l SSP_eCare_eCom"),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('SSP CSR POS') {
                    when { 
                        expression { return params.SSP_eCSR } 
                    }
                    steps {
                        echo "Running SSP CSR POS installation: ${ssp_ecsr_patches}"
                        build job: "${installer_subfolder}/Install_SSP", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: ssp_ecsr_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'ansible_extra_args', value: "-l SSP_CSR_POS"),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('SSP Customer POS') {
                    when { 
                        expression { return params.SSP_eCPOS } 
                    }
                    steps {
                        echo "Running SSP Customer POS installation: ${ssp_ecpos_patches}"
                        build job: "${installer_subfolder}/Install_SSP", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: ssp_ecpos_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'ansible_extra_args', value: "-l SSP_Customer_POS"),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }
                stage('TBAPI') {
                    when { 
                        expression { return params.TBAPI } 
                    }
                    steps {
                        echo "Running TBAPI installation: ${tbapi_patches}"
                        build job: "${installer_subfolder}/Install_TBAPI", 
                        parameters: [
                            string(name: 'configuration_git', value: environment_url),
                            string(name: 'configuration_branch', value: getBranch()),
                            string(name: 'configuration_id', value: "configuration"),
                            string(name: 'patches', value: tbapi_patches.replaceAll('172.23.253.40','rkt2040')),
                            string(name: 'INSTALLER', value: "${installer_link}"),
                            string(name: 'ssh_id', value: "toms_ssh")
                        ]
                    }
                }                
            }
        }
    }
}
