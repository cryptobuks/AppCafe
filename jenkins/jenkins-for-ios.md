
# Jenkins for iOS

Try to make iOS building by Jenkins.

# How to

1.It's better for your team to use [Git](https://git-scm.com/) to control your code,and also better for you to use [GitLab](https://about.gitlab.com/) for you code control system.

2.If you use Git and GitLab for your code management,let's start to use Jenkins to build your iOS project automatically.

3.Install these plugins:`Jenkins` -> `Manage Jenkins` -> `Manage Plugins` -> `Available`  
GitLab:`GitLab` & `Gitlab Hook`  
Xcode:  
`Xcode integration`  
`Keychains and Provisioning Profiles Management`  
And then install them without restart.

4.You must make your all file visible,it's easier for you to find some directories and files.  
`defaults write com.apple.finder AppleShowAllFiles -bool true`

# Tips

0.So I recommend you don't install Jenkins by `.pkg` installation file,and you can install Jenkins by `brew install jenkins-lts`,because you may meet this error which really not easy to solve.  
```
FATAL: Failed to copy /Users/Shared/Jenkins/Home/kpp_upload/iOS_Distribution_Ad_Hoc_20180802.mobileprovision to /Users/ifeegoo/Library/MobileDevice/Provisioning Profiles/6bc16df2-1086-4671-9125-a97542632ccc.mobileprovision
java.nio.file.AccessDeniedException: /Users/ifeegoo/Library/MobileDevice
	at sun.nio.fs.UnixException.translateToIOException(UnixException.java:84)
	at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:102)
	at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:107)
	at sun.nio.fs.UnixFileSystemProvider.checkAccess(UnixFileSystemProvider.java:308)
	at java.nio.file.Files.createDirectories(Files.java:746)
	at hudson.FilePath.mkdirs(FilePath.java:3098)
	at hudson.FilePath.write(FilePath.java:2002)
	at hudson.FilePath.copyTo(FilePath.java:2122)
Caused: java.io.IOException: Failed to copy /Users/Shared/Jenkins/Home/kpp_upload/iOS_Distribution_Ad_Hoc_20180802.mobileprovision to /Users/ifeegoo/Library/MobileDevice/Provisioning Profiles/6bc16df2-1086-4671-9125-a97542632ccc.mobileprovision
	at hudson.FilePath.copyTo(FilePath.java:2126)
	at com.sic.plugins.kpp.KPPProvisioningProfilesBuildWrapper.copyProvisioningProfiles(KPPProvisioningProfilesBuildWrapper.java:161)
	at com.sic.plugins.kpp.KPPProvisioningProfilesBuildWrapper.setUp(KPPProvisioningProfilesBuildWrapper.java:99)
	at hudson.model.Build$BuildExecution.doRun(Build.java:157)
	at hudson.model.AbstractBuild$AbstractBuildExecution.run(AbstractBuild.java:504)
	at hudson.model.Run.execute(Run.java:1798)
	at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:43)
	at hudson.model.ResourceController.execute(ResourceController.java:97)
	at hudson.model.Executor.run(Executor.java:429)
Finished: FAILURE
```  
The above error is terrible,because this is the file access permission problem.When you install Jenkins by `.pkg` file,Jenkins will be installed in the `Shared` diectory,but Jenkins will be installed in the `Users/{user}` directory,there will be no error the same as the above.

1.When you use Xcode command you meet this error:  
```
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```  
You need to use the command to solve this problem.  
```
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/
```

2.When you try to export .ipa file,you meet this error:  
```
FATAL: The path to store mobile provisioning profile files on the master is not configured. Go the plugin main configuration page and give the path.
java.io.IOException: The path to store mobile provisioning profile files on the master is not configured. Go the plugin main configuration page and give the path.
	at com.sic.plugins.kpp.KPPProvisioningProfilesBuildWrapper.copyProvisioningProfiles(KPPProvisioningProfilesBuildWrapper.java:142)
	at com.sic.plugins.kpp.KPPProvisioningProfilesBuildWrapper.setUp(KPPProvisioningProfilesBuildWrapper.java:99)
	at hudson.model.Build$BuildExecution.doRun(Build.java:157)
	at hudson.model.AbstractBuild$AbstractBuildExecution.run(AbstractBuild.java:504)
	at hudson.model.Run.execute(Run.java:1798)
	at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:43)
	at hudson.model.ResourceController.execute(ResourceController.java:97)
	at hudson.model.Executor.run(Executor.java:429)
Finished: FAILURE
```  
You must input your Provisioning Profiles directory in the plugin `Keychains and Provisioning Profiles Management`:  
Provisioning Profiles Directory Path:`/Users/ifeegoo/Library/MobileDevice/Provisioning Profiles`  

3.When you try to export .ipa file, you meet this error:  
```
+ xcodebuild -exportArchive -archivePath /Users/ifeegoo/Desktop/JenkinsTest.xcarchive -exportPath /Users/ifeegoo/Desktop/JenkinsTest.ipa -exportOptionsPlist /Users/ifeegoo/workspace/jenkins/JenkinsTest/ExportOptions.plist
2018-08-02 22:08:01.091 xcodebuild[3119:139257] [MT] IDEDistribution: -[IDEDistributionLogging _createLoggingBundleAtPath:]: Created bundle at path '/var/folders/vl/nvypcrfj25n20fhnmtl0t5p40000gn/T/JenkinsTest_2018-08-02_22-08-01.090.xcdistributionlogs'.
2018-08-02 22:08:01.460 xcodebuild[3119:139257] [MT] IDEDistribution: Step failed: <IDEDistributionPackagingStep: 0x7fed271c5c70>: Error Domain=NSCocoaErrorDomain Code=3840 "No value." UserInfo={NSDebugDescription=No value., NSFilePath=/var/folders/vl/nvypcrfj25n20fhnmtl0t5p40000gn/T/ipatool-json-filepath-bPPmdW}
error: exportArchive: The data couldn’t be read because it isn’t in the correct format.

Error Domain=NSCocoaErrorDomain Code=3840 "No value." UserInfo={NSDebugDescription=No value., NSFilePath=/var/folders/vl/nvypcrfj25n20fhnmtl0t5p40000gn/T/ipatool-json-filepath-bPPmdW}

** EXPORT FAILED **

Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

You must set the `compileBitcode` value to `false` in your ExportOptions.plist file.  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>compileBitcode</key>
	<false/>
	<key>method</key>
	<string>ad-hoc</string>
	<key>provisioningProfiles</key>
	<dict>
		<key>com.ifeegoo.JenkinsTest</key>
		<string>iOS Distribution Ad Hoc 20180802</string>
	</dict>
	<key>signingCertificate</key>
	<string>07FAC9A73462813682EBED256C631DAE8506B9D8</string>
	<key>signingStyle</key>
	<string>manual</string>
	<key>stripSwiftSymbols</key>
	<true/>
	<key>teamID</key>
	<string>AXTESR7SA4</string>
	<key>thinning</key>
	<string>&lt;none&gt;</string>
</dict>
</plist>
```
