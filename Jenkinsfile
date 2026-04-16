// node {

//     def PROD_HOST = "172.21.29.77"
//     def PROD_USER = "ubuntu"
//     def PROD_PATH = "/home/ubuntu/prod.kelasdevops.xyz"

//     stage('Checkout') {
//         checkout scm
//     }

//     stage('Build') {
//         docker.image('composer:2.6').inside('-u root') {
//             sh '''
//             composer install --ignore-platform-req=ext-gd --no-dev --optimize-autoloader
//             '''
//         }
//     }

//     stage('Testing') {
//         docker.image('ubuntu:22.04').inside('--entrypoint="" -u root') {
//             sh 'echo "Ini adalah test"'
//         }
//     }

//     stage('Deploy') {
//         docker.image('agung3wi/alpine-rsync:1.1').inside('--entrypoint="" -u root') {
//             sshagent(credentials: ['ssh-prod']) {
//                 sh """
//                 rsync -rav --delete -e "ssh -o StrictHostKeyChecking=no" ./ \
//                     ${PROD_USER}@${PROD_HOST}:${PROD_PATH}/ \
//                     --exclude='public/build' \
//                     --exclude='node_modules' \
//                     --exclude='vendor' \
//                     --exclude='storage' \
//                     --exclude='.git' \
//                     --exclude='.env'
//                 """
//             }
//         }
//     }
// }
node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
    sh '''
    whoami
    id
    ls -l /var/run/docker.sock

    docker run --rm \
      --user root \
      -v $(pwd):/app \
      -w /app \
      composer:2.6 \
      composer install --ignore-platform-req=ext-gd --no-dev --optimize-autoloader
    '''
}
    

}
