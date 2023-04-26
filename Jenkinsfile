pipeline {
  agent {
    label 'masternodes'
  }
  parameters {
    string(defaultValue: '', description: 'Enter the package name for ${params.PACKAGE_NAME} installation', name: 'PACKAGE_NAME')
    //    choice(choices: ['start', 'stop'], description: 'Choose service status', name: 'SERVICE_STATUS')
  }
  stages {
    stage('Check ${params.PACKAGE_NAME} and Install') {
      steps {
        script {
          check_package()
        }
      }
    }
    // stage('Start PostgreSQL') {
    //     steps {
    //         script {
    //             start_postgresql()
    //         }
    //     }
    // }
    // stage('Enable PostgreSQL') {
    //     steps {
    //         script {
    //             enable_postgresql()
    //         }
    //     }
    // }
  }
}

def check_package() {
  def installed = sh(returnStatus: true, script: "dpkg -s ${params.PACKAGE_NAME} | grep 'Status: install ok installed'")
  if (installed == 0) {
    echo "${params.PACKAGE_NAME} is already installed"
  } else {
    echo "Installing package: ${params.PACKAGE_NAME}"
    sh "sudo apt update && sudo apt install -y ${params.PACKAGE_NAME}"
    echo "Installation completed successfully for ${params.PACKAGE_NAME}."
    start_package()
    enable_package()
  }
}

def start_package() {
  echo "Starting ${params.PACKAGE_NAME} service..."
  def installed = sh(returnStatus: true, script: "systemctl is-active ${params.PACKAGE_NAME}")
  if (installed != "active") {
    sh "sudo systemctl start ${params.PACKAGE_NAME}"
    echo "${params.PACKAGE_NAME} has started and running."
  } else {
    echo "${params.PACKAGE_NAME} is already running."
  }
}

def enable_package() {
  echo "Enabling ${params.PACKAGE_NAME} service..."
  def installed = sh(returnStatus: true, script: "systemctl is-enabled ${params.PACKAGE_NAME}")
  if (installed != "enabled") {
    sh "sudo systemctl enable ${params.PACKAGE_NAME}"
    echo "${params.PACKAGE_NAME} service successfully enabled."
  } else {
    echo "${params.PACKAGE_NAME} is already enabled."
  }
}
