# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OSX and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Prerequisites
1. EC2 Instance (Amazon Linux 2 does not support Jenkins. Link to article - https://community.jenkins.io/t/support-of-jenkins-on-amazon-linux-2/5110)
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java 17 should be installed. (java-openjdk11 has end-of-support date of 2024-09-30)


## Install Jenkins
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo so that we can update it easily in the future.
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo yum upgrade
   yum search java | grep java-17
   # Add required dependencies for the jenkins package
   sudo yum install java-17-amazon-corretto-devel.x86_64
   sudo yum install jenkins
   # Check where Java is installed
   sudo alternatives --config java # select which Java version is needed by entering the number
   # Setting JAVA_HOME path
   cd ~
   vi .bashrc
   # Add this line - JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64 above user specfic environment section
   # User specific environment
   # if ! [[ "$PATH" =~ "$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin:" ]]
   # then
   #     PATH="$HOME/.local/bin:$HOME/bin:$PATH:$JAVA_HOME/bin"
   # fi
   # export PATH
   # Save and exit
   source .bashrc
   java --version

   ### Start Jenkins
   ```sh
   # Start jenkins service
   service jenkins start

   # Setup Jenkins to start at boot,
   chkconfig jenkins on
   ```

   ### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://YOUR-SERVER-PUBLIC-IP:8080
   ```
  #### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"
