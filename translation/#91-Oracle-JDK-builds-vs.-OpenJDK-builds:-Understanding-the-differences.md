Donald Smith, Sr Director of Product Management in the Java Platform Group at Oracle announced last year that the company intended that “within a few releases there should be no technical differences between OpenJDK builds and Oracle JDK binaries. Are we there yet? Are there no technical differences between the two? He clears the air in a new blog post.<br/>
在Oracle(公司)的Java平台组任职产品管理经理的Donald Smith在去年宣称，公司计划在此后的几次发布内希望可以使得OpenJDK和Oracle JDK构建的二进制文件没有技术上的差别。我们现在是否处在这一步了呢？这两者之间没有技术上的差别了么？他在他新发表的博客上进行了表述。<br/>
At last year’s Java One, Mark Cavage announced that [Oracle will open source Oracle JDK’s proprietary features](https://youtu.be/Tf5rlIS6tkg?t=567). Shortly before the conference, Donald Smith, Sr Director of Product Management in the Java Platform Group at Oracle wrote in a [blog post](https://blogs.oracle.com/java-platform-group/faster-and-easier-use-and-redistribution-of-java-se) that the company’s intent is that “within a few releases there should be no technical differences between OpenJDK builds and Oracle JDK binaries.”<br/>
在去年的Java One(译者注:“JavaOne 是每年一次的Java盛会，当然中国大陆现在也有了JavaChina，中国台湾有JavaTwo，而欧洲的JavaPolis则也是非常的壮观。 JavaOne是在Sun正式发布Java 1.0之后，Java这门新生的语言所拥有的自己的会议。”)盛会上，Mark Cavage宣称，[Oracle将开放Oracle JDK的专有特征的源代码](https://youtu.be/Tf5rlIS6tkg?t=567)。就在此次盛会的前不久，在Oracle(公司)的Java平台组任职产品管理经理的Donald Smith在他发表的[博客](https://blogs.oracle.com/java-platform-group/faster-and-easier-use-and-redistribution-of-java-se)中说，公司计划“在此后的几次发布内希望可以使得OpenJDK和Oracle JDK构建的二进制文件没有技术上的差别”。<br/>
Are we there yet? If you ask Donald Smith, the answer is yes but “with some cosmetic and packaging differences.” <br/>
我们现在是否处在这一步了呢？如果你这样问Donald Smith，那么他会回答是的，但是“会有一些表面上的和打包方面上的差异”。<br/>
# Oracle JDK builds vs. OpenJDK builds: The story so far <br/>
# Oracle JDK构建 VS OpenJDK构建：路还很长 <br/>
It’s been one year since it was first announced that there should be little to no technical difference between OpenJDK builds and Oracle JDK binaries but this conversation shows no signs of stopping.<br/>
距离第一次宣称“OpenJDK构建和OracleJDK构建之间不应该有技术上的差异”已经有一年了，但是此次对话并没有显示这种差异不会继续存在的迹象。<br/>
In September 2017, we invited Uwe Schindler, Apache Lucene PMC Member and Contributor of the OpenJDK Project, to weigh in on the proposed changes. Here’s what he said:<br/>
2017年9月，我们邀请了Apache Lucene PMC成员和OpenJDK项目的贡献者——Uwe Schindler来发表对于被提议的改变时，他说道：<br/>
> The Java Core has been the same for OpenJDK Builds and Oracle Builds since Java 7, so you could easily change between versions starting from Java 7. At least when working in server environments. So yes, one could say “Java is open source” to that.<br/>
自从Java 7之后对于OracleJDK和OpenJDK构建而言，Java核心是相似的，所以从Java 7之后你可以很容易地进行两者之间的更换。至少在服务器环境中工作时是没有差异的，对此可以说“Java是开源的”<br/>
Still, there have been differences concerning the implementation of some components. Foremost that concerned GUI-environments (AWT/Swing) where some parts were omitted in OpenJDK (sound output, options for graphic editing).<br/>
此外，考虑到一些组件的实现还是有差异的。最重要的是在OpenJDK中GUI环境(AWT/Swing)的一部分是被忽略的(譬如：输出，图形编辑选项)<br/>
**– Uwe Schindler**<br/>
**– Uwe Schindler**<br/>
## Read the full interview [here](https://jaxenter.com/is-java-truly-free-at-last-137285.html). 
## 在[此处](https://jaxenter.com/is-java-truly-free-at-last-137285.html)查看完整的访谈
One can, of course, choose to use Oracle JDK builds but since “Java is now developed as OpenJDK,” as Stephen Colebourne pointed out in a recent [blog post](https://blog.joda.org/2018/08/java-is-still-available-at-zero-cost.html), there are [more choices out there](https://blog.joda.org/2018/09/time-to-look-beyond-oracles-jdk.html).<br/>
但是自从“Java现在被发展为OpenJDK”人们当然能自己选择用Oracle JDK构建，正如Stephen Colebourne在他最近发表的一篇[博客](https://blog.joda.org/2018/08/java-is-still-available-at-zero-cost.html)中指出的那样，其实[还有很多选择](https://blog.joda.org/2018/09/time-to-look-beyond-oracles-jdk.html)。<br/>
> A key point to grasp is that most JDK builds in the world are based on the open source [OpenJDK project](http://openjdk.java.net/). The Oracle JDK is merely one of many builds that are based on the OpenJDK codebase. While it used to be the case that Oracle had additional extras in their JDK, as of Java 11 this is no longer the case.
<br/>需要抓住一个关键点就是在世界上大多数JDK构建都是基于[OpenJDK项目](http://openjdk.java.net/)的。Oracle JDK也仅仅是众多以OpenJDK代码为基础的构建之一。尽管在Oracle JDK中经常有额外的功能，但自从Java 11之后OpenJDK也有很多额外的功能了。<br/>
For the first 6 months of Java 11’s life, Oracle will be providing GPL+CE licensed $free zero-cost downloads at [jdk.java.net](http://jdk.java.net/) with security patches. To get GPL+CE licensed $free zero-cost update releases of Java 11 after the first six months, you are likely to need to obtain them from a different URL and a different build team.
<br/>在Java 11发布之后的半年里，Oracle将会提供GPL+CE许可免费下载安全补丁在[jdk.java.net](jdk.java.net)。在Java 11发布半年后获取免费更新的GPL+CE许可，你可能就需要从不同的URL或者不同的构建团队获取了。<br/>
** – Stephen Colebourne **<br/>
** – Stephen Colebourne **<br/>
## Read more about the costs involved [here](https://blog.joda.org/2018/08/java-is-still-available-at-zero-cost.html) and [here](https://blog.joda.org/2018/09/time-to-look-beyond-oracles-jdk.html). 
## 从[这里](https://blog.joda.org/2018/08/java-is-still-available-at-zero-cost.html)和[这里](https://blog.joda.org/2018/09/time-to-look-beyond-oracles-jdk.html)阅读更多关于花费的事

## We will explore this topic in-depth at [JAX London](https://jaxlondon.com/).
## 我们将在[JAX London](https://jaxlondon.com/)深度讨论这个话题。
![](https://jaxlondon.com/wp-content/uploads/2019/05/JAX_LDN19_Widget_Header_728x90_v1b.jpg)
![](https://jaxlondon.com/wp-content/uploads/2019/04/Dr.-Carola-Lilienthal-150x150.jpg)
[RESOLVING TECHNICAL DEBTS IN SOFTWARE ARCHITECTURE](https://jaxlondon.com/software-architecture-design/resolving-technical-debts-in-software-architecture/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=srticle_sessionbox)<br/>
[软件架构中解决技术上的债务](https://jaxlondon.com/software-architecture-design/resolving-technical-debts-in-software-architecture/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=srticle_sessionbox)<br/>
Dr. Carola Lilienthal (WPS - Workplace Solutions)<br/>
![](https://jaxlondon.com/wp-content/uploads/2019/04/Simon-Ritter-150x150.jpg)
[KEEPING UP WITH JAVA: LOOK AT ALL THESE NEW FEATURES!](https://jaxlondon.com/java-core-jvm-languages/keeping-up-with-java-look-at-all-these-new-features/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=article_sessionbox)<br/>
[跟上Java：看看这些全部的新特性！](https://jaxlondon.com/java-core-jvm-languages/keeping-up-with-java-look-at-all-these-new-features/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=article_sessionbox)<br/>
Simon Ritter (Azul Systems)<br/>
![](https://jaxlondon.com/wp-content/uploads/2019/04/Dr.-Daniel-Bryant-150x150.jpg)
[CLOUD NATIVE COMMUNICATION: USING AMBASSADOR AND CONSUL CONNECT WITH JAVA APPS](https://jaxlondon.com/cloud-kubernetes-serverless/cloud-native-communication-using-ambassador-and-consul-connect-with-java-apps/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=article_sessionbox)<br/>
[云原生交流：使用AMBASSADOR和CONSUL来连接Java应用](https://jaxlondon.com/cloud-kubernetes-serverless/cloud-native-communication-using-ambassador-and-consul-connect-with-java-apps/?utm_medium=banner&utm_source=jaxenter.com&utm_campaign=sands_om&utm_term=article_sessionbox)<br/>
Dr. Daniel Bryant (Big Picture Tech Ltd)<br/>
+ [JAX London Program »](https://jaxlondon.com/?utm_medium=referral&utm_source=jaxenter.com&utm_campaign=sessionbox&utm_term=article_sessionbox)

# Understanding the differences between Oracle JDK builds & OpenJDK builds
# 理解Oracle JDK和OpenJDK之间的一些差异
“[BCL](https://java.com/license)” license no more! Donald Smith explained in a recent [blog post](https://blogs.oracle.com/java-platform-group/oracle-jdk-releases-for-java-11-and-later) that JDK 11 will mark the beginning of a new era, one in which the tech giant offers “JDK releases under the open source [GNU General Public License v2, with the Classpath Exception (GPLv2+CPE)](http://openjdk.java.net/legal/gplv2+ce.html), and under a commercial license for those using the Oracle JDK as part of an Oracle product or service, or who do not wish to use open source software.”<br/>
不再是“[BCL](https://java.com/license)(binary code license)” 许可！Donald Smith在他最近发表的[博客](https://blogs.oracle.com/java-platform-group/oracle-jdk-releases-for-java-11-and-later)上解释道，JDK 11意味着一个新的纪元的开始，由巨头公司提供的“在[GPLv2+CE的开源许可](http://openjdk.java.net/legal/gplv2+ce.html)下的jdk发布，以及为了那些使用作为Oracle的一部分的产品和和服务的OracleJDK，或者不希望使用开源软件的那些人提供商业许可”。<br/>
> Different builds will be provided for each license, but these builds are functionally identical aside from some cosmetic and packaging differences.<br/>
针对每个许可都会有构建差异，但是除了表面上的和打包上的差异，这些构建在功能上是一致的。<br/>
**– Donald Smith**<br/>
**– Donald Smith**

From Java 11 -which will be released on [September 25](https://jaxenter.com/java-influencers-series-part-3-148682.html)– forward, Oracle JDK builds and [OpenJDK builds](http://jdk.java.net/) will be essentially identical, he added. However, there will be some “cosmetic and packaging differences.” 

## Here are the changes, as explained by Donald Smith:
+ Oracle JDK 11 emits a warning when using the -XX:+UnlockCommercialFeatures option, whereas in OpenJDK builds this option results in an error. This option was never part of OpenJDK and it would not make sense to add it now since there are no commercial features in OpenJDK. This difference remains in order to make it easier for users of Oracle JDK 10 and earlier releases to migrate to Oracle JDK 11 and later.
+ Oracle JDK 11 can be configured to provide usage log data to the “[Advanced Management Console](https://www.oracle.com/technetwork/java/javaseproducts/advanced-mgmt/advancedmanagementconsole-2254207.html)” tool, which is a separate commercial Oracle product.  We will work with other OpenJDK contributors to discuss how such usage data may be useful in OpenJDK in future releases, if at all.   This difference remains primarily to provide a consistent experience to Oracle customers until such decisions are made.
+ The javac –release command behaves differently for the Java 9 and Java 10 targets, since in those releases the Oracle JDK contained some additional modules that were not part of corresponding OpenJDK releases:
  + javafx.base
  + javafx.controls
  + javafx.fxml
  + javafx.graphics
  + javafx.media
  + javafx.web
  + java.jnlp
  + jdk.jfr
  + jdk.management.cmm
  + jdk.management.jfr
  + jdk.management.resource
  + jdk.packager.services
  + jdk.snmp

This difference remains in order to provide a consistent experience for specific kinds of legacy use. These modules are either now available separately as part of [OpenJFX](http://openjdk.java.net/projects/openjfx/), are now in both OpenJDK and the Oracle JDK because they were commercial features which Oracle contributed to OpenJDK (e.g., Flight Recorder), or were removed from Oracle JDK 11 (e.g., JNLP).

+ The output of the java –version and java -fullversion commands will distinguish Oracle JDK builds from OpenJDK builds, so that support teams can diagnose any issues that may exist.  Specifically, running java –version with an Oracle JDK 11 build results in:

java 11 2018-09-25

Java(TM) SE Runtime Environment 18.9 (build 11+28)

Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11+28, mixed mode)

## And for an OpenJDK 11 build:

openjdk version “11” 2018-09-25

OpenJDK Runtime Environment 18.9 (build 11+28)

OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)

+ The Oracle JDK has always required third party cryptographic providers to be signed by a known certificate.  The cryptography framework in OpenJDK has an open cryptographic interface, meaning it does not restrict which providers can be used.  Oracle JDK 11 will continue to [require](https://docs.oracle.com/javase/10/security/howtoimplaprovider.htm#JSSEC-GUID-2D4432F9-1C3C-4A91-8612-2B2840188B36) a valid signature, and Oracle OpenJDK builds will continue to allow the use of either a valid signature or unsigned third party crypto provider. 

+ Oracle JDK 11 will continue to include installers, branding and JRE packaging for an experience consistent with legacy desktop uses.

Oracle OpenJDK builds are currently available as zip and tar.gz files, while alternative distribution formats are being considered.

