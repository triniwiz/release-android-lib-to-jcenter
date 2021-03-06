# 发布lib到jcenter

## 准备
+ 注册账号：https://bintray.com (可以用github账号直接授权).
+ 注册完毕之后，记住用户名.
+ 在 `Edit Your Profile` -> `API Key` 中获取Key.
+ 确保自己本机 `Gradle` 的版本 `>=2.4`,否则会出错.

## 如何使用
+ 创建 `Android Library Project`
+ 在 `local.properties` 最后添加：
``` script
bintray.apikey=你的API Key
bintray.user=你的用户名
```
+ 修改 `根目录`build.gradle ,在 dependencies 中添加两句：
``` script
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.3.1'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
```
+ 还是修改 `根目录` build.gradle ,添加:
``` script
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())
```
并修改
``` script
allprojects {
    repositories {
    	// 新添加开始
    	maven {
            url properties.getProperty("sdk.dir")+"/extras/android/m2repository"
        }
        mavenLocal()
        // 新添加结束
        jcenter()
    }
}
```

+ 修改Module的build.gradle ，在最后添加：
``` script
ext {
	PUBLISH_GROUP_ID = '你的groupId'	// 填写groupId， 一般是包名，比如：com.android.support
	//PUBLISH_ARTIFACT_ID = '你的aritfactId'	// //这里不需要再填写，自动以Model的名字作为aritfactId
	PUBLISH_VERSION = '版本号'	// 版本号，比如：22.2.1
	PUBLISH_DES = '库的描述'   // 库的描述尽量不要用中文
	LIB_NAME = '你的lib名称'	// lib名称，比如：My_Lib
	
	WEBSITE_URL = '库的网站链接'	// 可以填写github上的库地址.
	ISSUE_TRACKER_URL = '库的issue链接'	// 可以填写github库的issue地址.
	VSC_URL = '库的版本控制地址'	// 可以填写github上库的地址.
}

// 下面这行请勿修改
apply from: 'https://raw.githubusercontent.com/andforce/release-android-lib-to-jcenter/master/bintray.gradle'
```

### 编译发布
``` script
gradle jcenter
```
### Add to JCenter
+ 执行完上面的步骤，你只是在bintray中创建了一个Package，要发布到JCenter还需要你手动去网站点一下`Add to JCenter`.之后等待审核就好了.

### 完整使用例子
https://github.com/andforce/Android-DevUtils

### 可能遇到的问题
#### 1. 如果你的库又引用了别的库怎么办？
> 不需要特殊处理，直接在 `dependencies` 中引用就可以了

#### 2. 因为现在是以Module的名字作为 `aritfactId` 如果想换怎么办？
> 只能修改 `Module` 的名称，具体需要修改3处：Module 路径名称，Module 名称， 以及settings.gradle中对应Module名称，都改成一致即可.



## 感谢:
https://github.com/dcendents/android-maven-gradle-plugin
https://github.com/bintray/gradle-bintray-plugin
http://theartofdev.com/2015/02/19/publish-android-library-to-bintray-jcenter-aar-vs-jar-and-optional-dependency/
http://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en
