 stage ('Smoke tests for Aurix') {
            steps {
                script {
                    libJenkins.when (enableSmokeTest) {
                        dir('smoke_test_main_high'){
                            parallelSmokeTest[''] = {                        
                                bat('pip install python-gitlab')
                                node('CGSCLXR81319686'){
                                    cleanWs()
                                    stage('checkout_Starter_kit_adas_IDC6_Main_High_ST3'){
                                        checkout([
                                        $class: 'GitSCM',
                                        branches: [[name: "Smoke_Test_for_Flash_Aurix"]],
                                        extensions: [
                                        [$class: 'CleanCheckout'],
                                        [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true, timeout: 60],
                                        [$class: 'CheckoutOption', timeout: 60]],
                                        userRemoteConfigs: [[
                                            credentialsId: 'adasdai-jenkins-ssh',
                                            url: 'ssh://git@git.swf.daimler.com:7999/adasvec/starter_kit_vswt.git'
                                        ]]
                                        ])
                                    }
                                    stage('Flashing Aurix Main_High_ST3'){
                                        // bat 'pip install artifactory'
                                        // bat 'pip install beautifulsoup4'
                                        // bat 'pip install pywin32'
                                        bat 'python devops\\smoke_test\\scripts\\flashing_execution\\download_Artifact.py'
                                        //bat 'python devops\\smoke_test\\scripts\\flashing_execution\\execute_rbs.py'
                                    }
                                    stage('Smoke Test Execution Main_High_ST3'){
                                        bat'''
                                        python devops\\smoke_test\\scripts\\flashing_execution\\execute_smoketest_rbs.py
                                        python devops\\smoke_test\\scripts\\flashing_execution\\terminate_rbs.py
                                        '''
                                    }
                                    stage('Upload Results to Artifactory Main_High_ST3'){
                                        bat '''python devops\\smoke_test\\scripts\\flashing_execution\\upload_Results.py'''
                                    }    
                                }
                            }
                        }       
                        dir('smoke_test_setellite'){
                            parallelSmokeTest[''] = {
                                bat('pip install python-gitlab')
                                node('CGSCLXR54392383'){
                                    cleanWs()
                                    stage('checkout_Starter_kit_adas_IDC6_Setellite_High_ST3'){
                                        checkout([
                                        $class: 'GitSCM',
                                        branches: [[name: "Smoke_Test_for_Flash_Aurix"]],
                                        extensions: [
                                        [$class: 'CleanCheckout'],
                                        [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true, timeout: 60],
                                        [$class: 'CheckoutOption', timeout: 60]],
                                        userRemoteConfigs: [[
                                            credentialsId: 'adasdai-jenkins-ssh',
                                            url: 'ssh://git@git.swf.daimler.com:7999/adasvec/starter_kit_vswt.git'
                                        ]]
                                        ])
                                    }
                                    stage('Flashing Aurix Setellite_High_ST3'){
                                        // bat 'pip install artifactory'
                                        // bat 'pip install beautifulsoup4'
                                        // bat 'pip install pywin32'
                                        bat 'python devops\\smoke_test\\scripts\\flashing_execution\\download_Artifact.py'
                                        //bat 'python devops\\smoke_test\\scripts\\flashing_execution\\execute_rbs.py'
                                    }
                                    stage('Smoke Test Execution Setellite_High_ST3'){
                                        bat'''
                                        python devops\\smoke_test\\scripts\\flashing_execution\\execute_smoketest_rbs.py
                                        python devops\\smoke_test\\scripts\\flashing_execution\\terminate_rbs.py
                                        '''
                                    }
                                    stage('Upload Results to Artifactory Setellite_High_ST3'){
                                        bat '''python devops\\smoke_test\\scripts\\flashing_execution\\upload_Results.py'''
                                    }    
                                }
                            }
                        }       
                        parallel(parallelSmokeTest)
                    }
                }
            }
        }




        -----------------------------------------------------------------------------------------------------------------------------



        stage ('Smoke tests for Aurix') {
    steps {
        script {
            libJenkins.when (enableSmokeTest) {
                parallelSmokeTest = [:] // Create a map to store the parallel stages

                // Stage 1: Smoke tests for Main_High_ST3
                parallelSmokeTest['Smoke Test for Main_High_ST3'] = {
                    node('CGSCLXR81319686') {
                        cleanWs()
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: "Smoke_Test_for_Flash_Aurix"]],
                            extensions: [
                                [$class: 'CleanCheckout'],
                                [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true, timeout: 60],
                                [$class: 'CheckoutOption', timeout: 60]],
                            userRemoteConfigs: [[
                                credentialsId: 'adasdai-jenkins-ssh',
                                url: 'ssh://git@git.swf.daimler.com:7999/adasvec/starter_kit_vswt.git'
                            ]]
                        ])

                        // Steps for Main_High_ST3
                        bat 'pip install python-gitlab'
                        // Other steps for Main_High_ST3
                    }
                }

                // Stage 2: Smoke tests for Setellite_High_ST3
                parallelSmokeTest['Smoke Test for Setellite_High_ST3'] = {
                    node('CGSCLXR54392383') {
                        cleanWs()
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: "Smoke_Test_for_Flash_Aurix"]],
                            extensions: [
                                [$class: 'CleanCheckout'],
                                [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true, timeout: 60],
                                [$class: 'CheckoutOption', timeout: 60]],
                            userRemoteConfigs: [[
                                credentialsId: 'adasdai-jenkins-ssh',
                                url: 'ssh://git@git.swf.daimler.com:7999/adasvec/starter_kit_vswt.git'
                            ]]
                        ])

                        // Steps for Setellite_High_ST3
                        bat 'pip install python-gitlab'
                        // Other steps for Setellite_High_ST3
                    }
                }

                // Execute the parallel stages
                parallel parallelSmokeTest
            }
        }
    }
}

