```text
       __           __   _                        __  ___________
      / /__  ____  / /__(_)___  _____            / / /_  __/ ___/
 __  / / _ \/ __ \/ //_/ / __ \/ ___/  ______   / /   / /  \__ \ 
/ /_/ /  __/ / / / ,< / / / / (__  )  /_____/  / /___/ /  ___/ / 
\____/\___/_/ /_/_/|_/_/_/ /_/____/           /_____/_/  /____/
```
## Introduction
> Jenkins is an open-source automation server with numerous plugins that facilitate Continuous integration/delivery and deployment (CI/CD). It runs in servlet containers, and it is used to build and test software. 
> Jenkins allows organizations to accelerate and automate the software development process. The automation makes it easier for developers to integrate changes and users to obtain fresh builds.

## Prerequisites
<ol>
    <li>A computer running macOS (or see how to Install Jenkins on Windows, Debian 10 (Buster), CentOS 8, Ubuntu 18.04).</li>
    <li>Java installed.</li>
    <li>An account with administrator privileges.</li>
</ol>

### 1. Install Jenkins Using the Homebrew Package Manager

- Step 1: Run command below to install `jenkins-lts`
    ```bash
    $ brew install jenkins-lts
    ```

- Step 2: To start jenkins server
    ```bash
    $ brew services start jenkins-lts
    ==> Successfully started `jenkins-lts` (label: homebrew.mxcl.jenkins-lts)
    ```
  - By default, Jenkins runs on port 8080. You can change port `8080` to other port in file `homebrew.mxcl.jenkins-lts.plist` in directory `/opt/homebrew/Cellar/jenkins-lts/{jenkins-version}` as below:
    ```bash
    $ cd opt/homebrew/Cellar/jenkins-lts/2.426.3
    $ vim homebrew.mxcl.jenkins-lts.plist
    ```
    - jenkins-lts.plist file after changed
    ```bash
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
            <key>Label</key>
            <string>homebrew.mxcl.jenkins-lts</string>
            <key>LimitLoadToSessionType</key>
            <array>
                    <string>Aqua</string>
                    <string>Background</string>
                    <string>LoginWindow</string>
                    <string>StandardIO</string>
                    <string>System</string>
            </array>
            <key>ProgramArguments</key>
            <array>
                    <string>/opt/homebrew/opt/openjdk/bin/java</string>
                    <string>-Dmail.smtp.starttls.enable=true</string>
                    <string>-jar</string>
                    <string>/opt/homebrew/opt/jenkins-lts/libexec/jenkins.war</string>
                    <string>--httpListenAddress=127.0.0.1</string>
                    <string>--httpPort=8070</string>
            </array>
            <key>RunAtLoad</key>
            <true/>
            <key>EnvironmentVariables</key>
            <dict>
            <key>PATH</key>
            <string>/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Docker.app/Contents/Resources/bin/:/Users/tayluong/Library/Group\ Containers/group.com.docker/Applications/Docker.app/Contents/Resources/bin</string>
            </dict>
    </dict>
    </plist>
    ```
  
  - Restart jenkins server to apply new port
    ```bash
    $ brew services restart jenkins-lts`
    ```

- Step 3: Unlock Jenkins

  - Get jenkins initialAdminPassword
    ```bash
    $ cat /Users/{user}/.jenkins/secrets/initialAdminPassword
    ```

  - Unlock: access http://localhost:8070/ active jenkins by administrator password

- Step 4: Config Jenkins
  [How to config Jenkins on Mac](https://phoenixnap.com/kb/install-jenkins-on-mac)

---
Reference sources:
 - [How to Install Jenkins on Mac](https://phoenixnap.com/kb/install-jenkins-on-mac)
