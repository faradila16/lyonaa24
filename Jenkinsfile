// node {
//     checkout scm
//     stage("Build"){
//         docker.image('composer:2.6').inside('-u root') {
//             sh 'rm -f composer.lock'
//             sh 'composer install'
//         }
//     }
//     stage("Testing"){
//         docker.image('ubuntu').inside('-u root') {
//             sh 'echo "Ini adalah test"'
//         }
//     }
//     stage("Deploy"){
//         // deploy env prod
//         docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
//             sshagent(credentials: ['ssh-prod']) {
//                 sh 'mkdir -p ~/.ssh'
//                 sh 'ssh-keyscan -H "172.21.29.77" > ~/.ssh/known_hosts'
//                 sh "rsync -rav --delete ./ ubuntu@172.21.29.77:/home/ubuntu/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git"
//             }
//         }
//     }
// }




node {
    // Definisikan variabel di level node agar bisa diakses semua stage
    def PROD_HOST = "172.21.29.77"
    def PROD_USER = "ubuntu"
    def PROD_PATH = "/home/ubuntu/prod.kelasdevops.xyz"

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        // Tips: Gunakan image composer resmi agar build lebih cepat (tidak perlu install manual)
        docker.image('composer:2.6').inside('-u root') {
            sh '''
            composer install --ignore-platform-req=ext-gd --no-dev --optimize-autoloader
            '''
        }
    }

    stage('Testing') {
        docker.image('ubuntu:22.04').inside('--entrypoint="" -u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage('Deploy') {
        docker.image('agung3wi/alpine-rsync:1.1').inside('--entrypoint="" -u root') {
            sshagent(credentials: ['ssh-prod']) {
                sh """
                # Hapus cache lama dengan mengabaikan Host Key Checking
                #ssh -o StrictHostKeyChecking=no ${PROD_USER}@${PROD_HOST} "rm -f ${PROD_PATH}/bootstrap/cache/packages.php ${PROD_PATH}/bootstrap/cache/services.php"

                # Jalankan rsync dengan mengabaikan Host Key Checking
                rsync -rav --delete -e "ssh -o StrictHostKeyChecking=no" ./ \
                    ${PROD_USER}@${PROD_HOST}:${PROD_PATH}/ \
                    --exclude='public/build' \
                    --exclude='node_modules' \
                    --exclude='vendor' \
                    --exclude='storage' \
                    --exclude='.git' \
                    --exclude='.env'
              
                """
                sh """
           #  ssh -o StrictHostKeyChecking=no ${PROD_USER}@${PROD_HOST} '
             #    cd ${PROD_PATH}
               #  docker-compose down
                # docker-compose up -d
           # '
       #  """
            }
        }
    }
}
