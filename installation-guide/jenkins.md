# Jenkins

The leading open source automation server, Jenkins provides hundreds of plugins to support building, deploying and automating any project.

# Installation

**A**.You can download the Jenkins here:[Jenkins LTS](https://jenkins.io/download/),and then install the `.pkg` file.

**B**.You can also install the Jenkins LTS by brew command:  
`brew install jenkins-lts`

Following steps by steps:

1.Unlock Jenkins,the installation guide will tell you where to get the admin password,for example:  
`/Users/Shared/Jenkins/Home/secrets/initialAdminPassword`  
You can use the command to get the password:  
`ifeegoo:~ ifeegoo$ sudo cat /Users/Shared/Jenkins/Home/secrets/initialAdminPassword
`  
2.Input the password and then go on,and then install the recommended plugins.  
3.And then generate the admin information.  
4.Let the Jenkins URL be: `http://localhost:8080`.



# Tips

1.If you want to get the admin password,it shows the permission denied,you can also try this command:  
`sudo chown -R $(whoami) /Users/Shared/Jenkins/Home/secrets`  

2.If you are not using English as the default language,please install the `Locale` plugin,and make the default language as English in Jenkins Config System and in the plugin Locale by set the value as `en-US`.  
Remember:make `Ignore browser preference and force this language to all users` selected.  
**English language will make you more global as a developer!**

3.There is an easy way for you to restart the Jenkins with the URL:  
`http://localhost:8080/` + `restart`

4.If you install Jenkins LTS by brew command and you may meet this problem:  
` "This Jenkins instance appears to be offline" and offers options to "Configure Proxy" or "Skip Plugin Installation"`  
You can solve this kind of problem as follows:  
```
<?xml version='1.1' encoding='UTF-8'?>
<sites>
  <site>
    <id>default</id>
    <url>https://updates.jenkins.io/update-center.json</url>
  </site>
</sites>
```  
Change `https` to `http`:  
`<url>http://updates.jenkins.io/update-center.json</url>`

5.If you try to install the plugins recommended,while error occurs:  
`An error occurred during installation: No such plugin: cloudbees-folder`  
Try to restart Jenkins by `http://localhost:8080/restart`  

6.If you try to install the plugins and while time-out errors occur,maybe your internet is not stable,and you can also restart your Jenkins,or install plugins by uploading the installation packages
