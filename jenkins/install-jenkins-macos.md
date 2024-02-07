- Install Jenkins on macOS
https://phoenixnap.com/kb/install-jenkins-on-mac

- Set up pipeline for project
https://howtodoinjava.com/devops/setup-jenkins-pipeline-for-spring-boot-app/

- Allow docker run in Jenkins
https://harshityadav95.medium.com/how-to-setup-docker-in-jenkins-on-mac-c45fe02f91c5

- LINUX: $ sudo usermod -a -G docker jenkins

```
$ cd /opt/homebrew/Cellar/jenkins-lts/2.426.2
$ vim homebrew.mxcl.jenkins-lts.plist 
```

- Replace content as below:
```
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
                <string>--httpPort=8080</string>
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