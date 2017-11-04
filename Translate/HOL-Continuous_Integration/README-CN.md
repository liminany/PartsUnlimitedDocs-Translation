# 动手实验 - 通过使用TFS(VSTS) ，为 Parts Unlimited WebSite 项目 添加持续集成 

在这次实验中，我们采用的项目叫作：PartsUnlimited. 我们通过配置TFS(VSTS) ，可以为主分支代码增加 持续集成 的能力. 也就是说，在我们的代码提交和推送给主分支后，我想确保代码被正确集成，并可以快速获得反馈. 为此, 我们需要配置 我们的构建来完成持续集成  (CI)，使我们的代码一旦提交到TFS(VSTS)，可以自动触发编异、单元测试等动作作.

## 准备工作（TFS/VSTS）
- 完成练习： [Getting Started](../GettingStarted.md) .
-  一个可用的 TFS(VSTS) 帐号.

	 - 可参考的文档：　[如何 注册和设置 TFS](#)　　　　　[如何 注册和设置 Visual Studio Team Services](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) 

NOTE: 上面的帮助文档里面的图片可能已经过期，需要更新，但不影响阅读. they are left as is for not to provide some guidance if it is needed.

## 练习任务概述: 

**1. 用你的TFS(VSTS)帐号导入源码:** 在这个步骤,你将使用自己的帐号连接到TFS(VSTS)， 下载项目 PartsUnlimited 的郑代码，通过练习后，可以推送代码到你TFS(VSTS)代码仓库. 有两种方式来完成: a) 使用Git 命令行工具,  b) 或者使用宇届第一强IDE： Visual Studio 2017.  

Note: 题外话：TFS/VSTS 目前还不支持通过 VS IDE来创建构建，一般我们通过TFS/VSTS Web门户来实现  

**2. 创建持续集成构建:** 在这个步骤, 你将定义一个生成，这个生成会在代码每次提交到TFS(VSTS)之后触发.

**3. 验证CI 的触发:** 在这个步骤, 我们通过修改 Parts Unlimited 项目的代码，并提交到TFS(VSTS)以触发我们定义好的构建.

## I. 第一部分: 使用Git从TFS/VSTS中导入代码

为了能够使用TFS(VSTS)的持续集成功能，我们先要把代码导入到自己的代码仓库中.

> **注意:** 这次练习我们采用Git的方式来管理源码. 在接下来的一系列步骤中，我们使用添加PartUnlimited 源码到Git主仓库.

如果你还没有这份代码，首先要在TFS(VSTS)中创建一个团队项目I，并且源码管理选择Git.

![](<media/empty-vsts-git.png>)

## 1. 克隆仓库代码到本地目录.

在你的本地系统磁盘中创建一个父目录：**Working Directory**. 比如，在Windows系统中你可以创建这样一个目录:

`C:\Source\Repos`

打开Git命令行，并且更改目录为上面创建的.

使用上面的命令克隆仓库. 你可以把步骤 1 复制的URL 粘贴过来.  在下面的示例中, 此次克隆的仓库代码会得到到 HOL目录中. 当然你也可以克隆到任意目录中，也可以留空，克隆到根目录:

	git clone https://github.com/Microsoft/PartsUnlimited.git HOL

过几分钟，代码应该会被下载到你本地电脑

进入刚刚创建的目录.  在Windows系统中 (并且确保你使用的是目录名为HOL), 你可以使用下面的命令:

	cd HOL

## 2. 移除对 GitHub 的链接.

刚刚下载的 Git仓库 会通过一个叫 _orgin_ 的配置项指向Github仓库.  我们不在使用Github来提交我们的代码，因此我们可以删除这个引用

要移除这个引用，我们可以使用以下命令：

	git remote remove origin

## 3. 找到TFS/VSTS Git 仓库的访问URL

首先，我们需要找到这个URL来清空我们的TFS/VSTS Git 仓库如果你记得自己的帐号, 并且团队项目已创建，这个默认的Git地址很容易得出:

	https://<account>.visualstudio.com\_git\<project>

当然, 你也可以通过浏览器打开你的帐号，进入你的项目，然后在点击 代码 标签页 来获取默认的Git仓库地址:

	https://<account>.visualstudio.com

另外, 在页面的底部, 你可以看到有两个命令，通过这些命令我们可以把刚才克隆的代码推送到TFS/VSTS.

![](<media/findVstsRepoUrl.png>)

## 4. 推送本地仓库到TFS/VSTS，并建立链接r

进入步骤1创建的本地目录, 使用下面的命令，在远程仓库创建立名为 _origin_的链接. 你也可以输入步骤3里面的URL地址, 或者从TFS/VSTS Web页面获取这个URL地址.

	git remote add origin https://<account>.visualstudio.com\<project>\_git\<project>
现在你可以推送代码了到TFS/VSTS，包括提交的历史记录:

	git push -u origin --all
Congratulations, your code should now be in VSTS!

## II. 第二部分创建构建

定义好的生成定义，使我们可以知道迁入的代码是否编异成功、是否通过自动化测试.

## 1. 打开 **我的帐户** 页面:

	https://<account>.visualstudio.com


## 2.** 点击 **浏览** ,选择团队项目，点击 **导航**.

![](<media/CI1.png>)

## 3. 进入主页面后, 点击页面顶部的 **生成** , 然后进入 **所有定义**, 最后点击 **添加生成定义**.

![](<media/CI2.png>)

## 4. 选择 **空定义** 创建生成定义，然后点击 **下一步**.

![](<media/CI3.png>)

**Note:** 如你所见, 你可以定义UWA通用应用、 Xamarin Android/IOS应用的构建，甚至是Xcode 的构建.

## 5. 然后点击 **下一步** :
- 选择 **HOL Team Project**;
- 选择 **HOL** Repository;
- 选择 **Master** 做为默认分支;
- 选择 **Continuous Integration**;
- 把 **Default Agent Queue** 改为 **Hosted VS2017**;
- 点击 **Create**.

<!--- Image needs to be update to highlight the change on the Default Agent Queue to **Hosted VS2017**.
![](<media/CI4.png>)
--->

**Note:** 我们通常会有多个仓库，每个仓库的代码都会有多个分支,因此在我们选择构建方案时，需要选择正确的仓库和分支.

## 6. 点击 **Create** 按扭, 在 **Build** tab 页，  **Add build step** 会出现在页面中. 

![](<media/CI5.png>)

## 7. 在 **Add tasks** 对话框中, 选择 **Utility** 页面，添加一个 **PowerShell** 任务.

![](<media/CI6.png>)

## 8. 还在 **Add tasks** 对话框中,选择 **Test** 页面，添加一个 **Publish Test Results** 任务.

 ![](<media/CI7.png>)

## 9. 选择 **Utility** 页面，添加一个**Copy and Publish Artifacts** 任务，最后点击 **Close** 关闭对话框.

![](<media/CI8.png>)

## 10. 在 **PowerShell Script** 任务中, 点击蓝色的 **rename** 铅笔 图标,更改名称为 **dotnet restore, build, test and publish** ，然后点击 **OK**

## 11.  配置上面添加的任务:
- 为属性 **Type** 的值选择 **File Path** 
- 为属性 **Script filename** 设置值为 **"build.ps1"** 
- 为属性 **Arguments** 设置值为 **$(BuildConfiguration) $(System.DefaultWorkingDirectory) $(build.stagingDirectory)** .
- 点击高级，设置属性 **Working Folder** 值为 **$(System.DefaultWorkingDirectory)**

![](<media/CI9.1.png>)

<!--- Image needs to be updated to have the new parameters as argument.
 ![](<media/CI9.2.png>) 
--->

 **Note:** PS脚本**build.ps1** 使用 .Net Core 的  **dotnet.exe** 执行命令.  这些生成脚本执行: restore, build, test, publish, 等动作， 并生成一个  MSDeploy 格式的 zip包.

## 12. 在 **Publish Test Results** 任务中确保遵循以下修改:
- 修改 **Test Result Format**  为 **VSTest**.
- 修改 **Test Results File** 为 **\*\*/testresults.xml**.
- 修改 **Search folder** 为 **$(System.DefaultWorkingDirectory)**

<!--- Image needs update to show the correct Test Result Format option
![](<media/CI10.png>)
--->

## 13. 在 **Copy Publish Artifact** 任务中, 修改属性 **Copy Root** 为 **$(build.stagingDirectory)**, 修改 **Contents** 为 **\*\*\\\*.zip**, 修改 **Artifact Name** 为 **drop** ，设置 **Artifact Type** 为 **Server**.

![](<media/CI11.png>)

## 14** 在 **Variables** 页面，这些变量被用于build.ps1 PowerShell 脚本中; 添加新变量 **BuildConfiguration** ，值为 **release**.

![](<media/CI12.png>)

## 15. 点击 **Triggers** tab页面，确保 **Continuous integration (CI)** 被选中 . 同样确保 **master** 和 **Batch Changes** 未被选中 

![](<media/CI13.png>)

 **Note:**  为使持续集成生效，确保勾选 **Continuous integration (CI)** 一项. 当然你也可以根据项目的需要选择特定的分支代码进行持续集成.

## 16.** 点击 **Save**，并为生成定义设置一个名称.

![](<media/CI14.png>)

## III. 第三部分 测试CI 在TFS/VSTS中 被触发

We will now test the **Continuous Integration build (CI)** build we created by changing code in the Parts Unlimited project with Visual Studio Team Services.

## 1.** Select the **Code** hub and then select your your repo, **HOL**.

## 2. Navigate to **/src/PartsUnlimitedWebsite/Controllers** in the PartsUnlimited project, then click on the ellipsis to the right of **HomeController.cs** and click **Edit**.

![](<media/CI15.png>)

## 2. After clicking **Edit**, add in text (i.e. *This is a test of CI*) after the last *Using* statement. Once complete, click **Save**.

![](<media/CI16.png>)

## 3. Click **Build** hub. This should have triggered the build we previously created.

![](<media/CI17.png>)

## 4. Click on the **Build Number**, and you should get the build in progress. Here you can also see the commands being logged to console and the current steps that the build is on.
![](<media/CI17.1.png>)

## 4. Click on the **Build Number** on the top left and you should get a build summary similar to this, which includes test results.

![](<media/CI18.png>)

Next steps
----------

In this lab, you learned how to push new code to Visual Studio Team Services, setup a Git repo and create a Continuous
Integration build that runs when new commits are pushed to the master branch.
This allows you to get feedback as to whether your changes made breaking syntax
changes, or if they broke one or more automated tests, or if your changes are a
okay.

Try these labs out for next steps:

-[Continuous Deployment Lab](../HOL-Continuous_Deployment/README.md)
