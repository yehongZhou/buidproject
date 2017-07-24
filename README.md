[TOC]

### 常用打包脚本 

#### xcodebuild

```ruby
#打包为xcarchive
xcodebuild archive 
-workspace ${YourWorkspace.xcworkspace} 
-scheme ${YourScheme} 
-configuration Release 
-archivePath ${xcarchive path}

#导出IPA
xcodebuild -exportArchive 
-archivePath ${xcarchive path}'.xcarchive' 
-exportPath ${exportPath} 
-exportOptionsPlist 'exportOptions.plist'
```
exportOptions.plist的内容
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>method</key>
	<string>development</string>
</dict>
</plist>
```

#### fastlane

 安装
```
brew cask install fastlane
或者
sudo gem install fastlane -n /usr/local/bin
```

打包
```
#打包并导出为IPA
fastlane gym 
--workspace ${YourWorkspace.xcworkspace} 
--scheme ${YourScheme}  
--export_method 'development' 
--output_name ${PROJECT_NAME}'.ipa' 
--output_directory ${BUILD_PATH} 
--archive_path ${BUILD_PATH} 
--configuration 'Release' 
--clean true
```

#### xctool

安装
```
brew install xctool
```

 打包
```
#清理
xctool clean 
-workspace YourWorkspace.xcworkspace 
-scheme YourScheme 
-configuration 'Release'

#打包为xcarchive
xctool build 
-workspace YourWorkspace.xcworkspace 
-scheme YourScheme 
-configuration 'Release' archive 
-archivePath ${archive_path}

#导出用xcodebuild
```
### 快速使用
#### aggregate
```
1、新建target->cross-platform->aggregate
2、Build Phases->New Run Script Phase
```
Shell 内容

```
export LANG=en_US.UTF-8

PROJECT_NAME='SoloVideo'
BUILD_PATH='./build/'

#使用fastlane
#fastlane gym 
--workspace ${PROJECT_NAME}'.xcworkspace' 
--scheme ${PROJECT_NAME}  
--export_method 'development' 
--output_name ${PROJECT_NAME}'.ipa' 
--output_directory ${BUILD_PATH} 
--archive_path ${BUILD_PATH} 
--configuration 'Debug' 
--clean true

#fastlane简写语法
#fastlane gym 
-w ${PROJECT_NAME}'.xcworkspace' 
-s ${PROJECT_NAME}  
-j 'development' 
-n ${PROJECT_NAME}'.ipa' 
-o ${BUILD_PATH} 
-b ${BUILD_PATH} 
-q 'Debug' 
-c true

#使用xcodebuild
xcodebuild archive 
-workspace ${PROJECT_NAME}'.xcworkspace' 
-scheme ${PROJECT_NAME} 
-configuration Release 
-archivePath ${BUILD_PATH}${PROJECT_NAME}

xcodebuild -exportArchive 
-archivePath ${BUILD_PATH}${PROJECT_NAME}'.xcarchive' 
-exportPath ${BUILD_PATH} 
-exportOptionsPlist 'exportOptions.plist'

#上传蒲公英
#curl -F "file=@${BUILD_PATH}${PROJECT_NAME}.ipa" -F "uKey=${YourKey}" -F "_api_key=${YourApiKey}" https://qiniu-storage.pgyer.com/apiv1/app/upload

```

查看aggregate执行的日志
```
View->Navigators->Show Report Navigator
```
#### shell
```
项目目录下新建Pgyer.sh
```
Shell 内容

```shell
export LANG=en_US.UTF-8

PROJECT_NAME='SoloVideo'
BUILD_PATH='./build/'

#更新pod（能不更新就不更新）
pod update --no-repo-update

#使用fastlane 打包
#fastlane gym 
--workspace ${PROJECT_NAME}'.xcworkspace' 
--scheme ${PROJECT_NAME}  
--export_method 'development' 
--output_name ${PROJECT_NAME}'.ipa' 
--output_directory ${BUILD_PATH} 
--archive_path ${BUILD_PATH} 
--configuration 'Debug' 
--clean true

#fastlane简写语法
#fastlane gym 
-w ${PROJECT_NAME}'.xcworkspace' 
-s ${PROJECT_NAME}  
-j 'development' 
-n ${PROJECT_NAME}'.ipa' 
-o ${BUILD_PATH} 
-b ${BUILD_PATH} 
-q 'Debug' 
-c true

#使用xcodebuild 打包
xcodebuild archive 
-workspace ${PROJECT_NAME}'.xcworkspace' 
-scheme ${PROJECT_NAME} 
-configuration Release 
-archivePath ${BUILD_PATH}${PROJECT_NAME}

xcodebuild -exportArchive 
-archivePath ${BUILD_PATH}${PROJECT_NAME}'.xcarchive' 
-exportPath ${BUILD_PATH} 
-exportOptionsPlist 'exportOptions.plist'

#上传蒲公英
#curl -F "file=@${BUILD_PATH}${PROJECT_NAME}.ipa" -F "uKey=${YourKey}" -F "_api_key=${YourApiKey}" https://qiniu-storage.pgyer.com/apiv1/app/upload

```




