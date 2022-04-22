# Maven发布流程

[toc]

## 生成GPG密钥

- 登录 http://www.gnupg.org/，下载客户端并安装
- 使用客户端创建个人OpenPGP密钥对
  - 填写用户名、邮箱、密码等
  - 将公钥上传到目录服务
  - 密钥服务器地址可修改为hkp://keyserver.ubuntu.com:80

## 申请项目

- 登录 https://issues.sonatype.org，注册后，创建问题
  - 问题类别为`New Project`
  - 填写概要、描述、Group Id、Project URL（github项目地址）、SCM url（.git结尾的源码地址）
  - 验证Group Id，需修改域名的txt解析
- 等待问题通过后，即可进行下一步

## 配置settings.xml

- 配置server

  ```xml
  <server>
  	<id>sonatype-nexus-snapshots</id>
  	<username>sonatype账号</username>
  	<password>sonatype密码</password>
  </server>
  <server>
  	<id>sonatype-nexus-staging</id>
  	<username>sonatype账号</username>
  	<password>sonatype密码</password>
  </server>    
  <server>
  	<id>ossrh</id>
  	<username>sonatype账号</username>
  	<password>sonatype密码~</password>
  </server>
  ```

- 配置profile

  ```xml
  <profile>
  	<id>ossrh</id>
  	<activation>
  		<activeByDefault>true</activeByDefault>
  	</activation>
  	<properties>
  		<gpg.executable>C:/Program Files (x86)/GnuPG/bin/gpg.exe</gpg.executable>
  		<gpg.passphrase>gpg密钥密码</gpg.passphrase>
  		<gpg.homedir>C:/Users/zengyongli/AppData/Roaming/gnupg</gpg.homedir>
  	</properties>
  </profile>
  ```

  > homedir查询方法：进入上面bin目录，使用cmd：`gpg --list-key`查询

## 配置pom.xml

### 必须项

配置项目信息、许可信息、scm、开发者等信息：

```xml
<name>${project.groupId}:${project.artifactId}</name>
<description>A small and precise toolkit</description>
<url>https://github.com/zeoly/hiddenblade</url>

<licenses>
    <license>
        <name>The Apache License, Version 2.0</name>
        <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
    </license>
</licenses>

<scm>
    <connection>https://github.com/zeoly/hiddenblade.git</connection>
    <developerConnection>https://github.com/zeoly/hiddenblade</developerConnection>
    <url>https://github.com/zeoly/hiddenblade</url>
</scm>

<developers>
    <developer>
        <name>Zack Zeng</name>
        <email>zeoly100@163.com</email>
        <organization>Yahacode</organization>
        <organizationUrl>http://www.yahacode.com</organizationUrl>
    </developer>
</developers>
```

### 发布地址

配置snapshot与release的发布地址：

```xml
<distributionManagement>
    <snapshotRepository>
        <id>ossrh</id>
        <url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>
    </snapshotRepository>
    <repository>
        <id>ossrh</id>
        <url>https://s01.oss.sonatype.org/service/local/staging/deploy/maven2/</url>
    </repository>
</distributionManagement>
```

### 插件

包括源码、javadoc、gpg、nexus发布等插件：

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.2.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <goals>
                <goal>jar-no-fork</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>2.9.1</version>
    <executions>
        <execution>
            <id>attach-javadocs</id>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-gpg-plugin</artifactId>
    <version>1.5</version>
    <executions>
        <execution>
            <id>sign-artifacts</id>
            <phase>verify</phase>
            <goals>
                <goal>sign</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.sonatype.plugins</groupId>
    <artifactId>nexus-staging-maven-plugin</artifactId>
    <version>1.6.7</version>
    <extensions>true</extensions>
    <configuration>
        <serverId>ossrh</serverId>
        <nexusUrl>https://s01.oss.sonatype.org/</nexusUrl>
        <autoReleaseAfterClose>false</autoReleaseAfterClose>
    </configuration>
</plugin>
```

## 发布

- 开发完成后，可通过idea的deploy操作进行发布提交
- 如果是快照版本，地址在 https://s01.oss.sonatype.org/content/repositories/snapshots/
- 如果是发布版本，提交后，因为上面配置了`autoReleasAfterClose`为`false`，则会流转到收到发布环节。登录 https://s01.oss.sonatype.org/，可在`Stage Repository`中找到待发布包，确认没有问题后，点击`Release`进行发布
- 发布后，需等待约10分钟后，可在中央仓库查询到发布包：https://repo1.maven.org/maven2/
- 等待约4小时后，可在sonatype中搜索到发布包：https://search.maven.org/
