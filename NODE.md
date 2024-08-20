# $${\color{red} \textbf{NODE}}$$

${\color{green} \textbf{1. Launch Two instance one as MASTER and one as NODE}}$
- ubuntu
- t2.medium
- storage: 40

${\color{green} \textbf{2. On MASTER install jenkins and Access it }}$
````
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
````
${\color{purple} \textbf{ACCESS JENKINS : pub_ip:8080}}$

${\color{green} \textbf{3. On NODE Install Java }}$
````
sudo apt update
sudo apt install fontconfig openjdk-17-jre
````

${\color{green} \textbf{4. Generate SSH key }}$
````
cd .ssh/
````
````
ssh-keygen
````
${\color{purple} \textbf{NOTE :}}$ Copy public key for NODE.

${\color{green} \textbf{5. Paste that puplic key in NODE in authorized_keys file}}$
````
vim .ssh/authorized_keys
````
${\color{green} \textbf{6. Check connection from MASTER}}$
````
ping <private_ip_of_NODE>
````
${\color{green} \textbf{7. Go to jenkins and Add credential }}$
- Manage jenkins -> Credential
- global
- Add credential
- kind : ssh username with private key
- ID : ssh
- Description : ssh
- username : ubuntu
- Add Private key of MASTER
  ````
  cat .ssh/id_ed25519
  ````
- Enter directly (add that private ip)

${\color{green} \textbf{8. Create NODE in jenkins}}$
- Name : NODE1
- Permanent agent
- No of jobs : 2
- Add Remote root Directory : /opt/jenkins
- Label : test
- Launch Method : Launch agent via ssh
- HOST : add private ip of node
- Add credential
- Host key verification strategy : Non verifying verification strategy.
- SAVE

${\color{green} \textbf{9. Create pipeline }}$
- test-node
- pipeline
- Add pipeline code
````
pipeline {
    agent any

    stages {
        stage('create dir') {
            steps {
                sh 'mkdir guru'
            }
        }
    }
}
````
- apply & save
- Build now 
