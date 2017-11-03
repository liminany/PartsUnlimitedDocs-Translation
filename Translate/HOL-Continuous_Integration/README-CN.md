动手实验 - 通过使用TFS(VSTS) ，为 Parts Unlimited WebSite 项目 添加持续集成 
====================================================================================
在这次实验中，我们采用的项目叫作：PartsUnlimited. 我们通过配置TFS(VSTS) ，可以为主分支代码增加 持续集成 的能力. 也就是说，在我们的代码提交和推送给主分支后，我想确保代码被正确集成，并可以快速获得反馈. 为此, 我们需要配置 我们的构建来完成持续集成  (CI)，使我们的代码一旦提交到TFS(VSTS)，可以自动触发编异、单元测试等动作作.

### 准备工作（TFS/VSTS）: ###
- 完成练习： [Getting Started](../GettingStarted.md) .
-  一个可用的 TFS(VSTS) 帐号.

	   [如何 注册和设置 TFS](#)            [如何 注册和设置 Visual Studio Team Services](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) 

NOte: 上面的帮助文档里面的图片可能已经过期，需要更新，但不影响阅读. they are left as is for not to provide some guidance if it is needed.

### 练习任务概述: ###

**1. 用你的TFS(VSTS)帐号导入源码:** 在这个步骤,你将使用自己的帐号连接到TFS(VSTS)， 下载项目 PartsUnlimited 的郑代码，通过练习后，可以推送代码到你TFS(VSTS)代码仓库. 有两种方式来完成: a) 使用Git 命令行工具,  b) 或者使用宇届第一强IDE： Visual Studio 2017.  

> Note: 题外话：TFS/VSTS 目前还不支持通过 VS IDE来创建构建，一般我们通过TFS/VSTS Web门户来实现  

**2. 创建持续集成构建:** 在这个步骤, 你将定义一个生成，这个生成会在代码每次提交到TFS(VSTS)之后触发.

**3. 验证CI 的触发:** 在这个步骤, 我们通过修改 Parts Unlimited 项目的代码，并提交到TFS(VSTS)以触发我们定义好的构建.

### I: 使用Git从TFS/VSTS中导入代码

为了能够使用TFS(VSTS)的持续集成功能，我们先要把代码导入到自己的代码仓库中.

> **注意:** 这次练习我们采用Git的方式来管理源码. 在接下来的一系列步骤中，我们使用添加PartUnlimited 源码到Git主仓库.

如果你还没有这份代码，首先要在TFS(VSTS)中创建一个团队项目I，并且源码管理选择Git.

![](<media/empty-vsts-git.png>)

**1.** 克隆仓库代码到本地目录.

在你的本地系统磁盘中创建一个父目录：**Working Directory**. 比如，在Windows系统中你可以创建这样一个目录:

`C:\Source\Repos`

打开Git命令行，并且更改目录为上面创建的.

使用上面的命令克隆仓库. 你可以把步骤 1 复制的URL 粘贴过来.  在下面的示例中, 此次克隆的仓库代码会得到到 HOL目录中. 当然你也可以克隆到任意目录中，也可以留空，克隆到根目录:

	git clone https://github.com/Microsoft/PartsUnlimited.git HOL

过几分钟，代码应该会被下载到你本地电脑

进入刚刚创建的目录.  在Windows系统中 (并且确保你使用的是目录名为HOL), 你可以使用下面的命令:

	cd HOL

**2.** 移除对 GitHub 的链接.

刚刚下载的 Git仓库 会通过一个叫 _orgin_ 的配置项指向Github仓库.  我们不在使用Github来提交我们的代码，因此我们可以删除这个引用

要移除这个引用，我们可以使用以下命令：

	git remote remove origin

**3.** 找到TFS/VSTS Git 仓库的访问URL

首先，我们需要找到这个URL来清空我们的TFS/VSTS Git 仓库如果你记得自己的帐号, 并且团队项目已创建，这个默认的Git地址很容易得出:

	https://<account>.visualstudio.com\_git\<project>

当然, 你也可以通过浏览器打开你的帐号，进入你的项目，然后在点击 代码 标签页 来获取默认的Git仓库地址:

	https://<account>.visualstudio.com

另外, 在页面的底部, 你可以看到有两个命令，通过这些命令我们可以把刚才克隆的代码推送到TFS/VSTS.

![](<media/findVstsRepoUrl.png>)

**4.** Add the link to VSTS and push your local Git repo

In the local directory from Step 1, use the following command to add VSTS as the Git remote named _origin_. You can either type the URL you found in Step 3, or simply copy the first command from the VSTS web page.

	git remote add origin https://<account>.visualstudio.com\<project>\_git\<project>
Now you can push the code, including history, to VSTS:

	git push -u origin --all
Congratulations, your code should now be in VSTS!

### II. Create Continuous Integration Build

A continuous integration build will give us the ability check whether the code
we checked in can compile and will successfully pass any automated tests that we
have created against it.

**1.** Go to your **account’s homepage**:

	https://<account>.visualstudio.com


**2.** Click **Browse** and then select your team project and click
**Navigate**.

![](<media/CI1.png>)

**3.** Once on the project’s home page, click on the **Build** hub at the top of the page, then on **All Definitions**, and then on **New Definition**.

![](<media/CI2.png>)

**4.** Select the **Empty** build definition, and then click **Next**.

![](<media/CI3.png>)

>**Note:** As you can see, you can now do Universal Windows Apps & Xamarin Android/IOS Builds as well as Xcode builds.

**5.** After clicking the **Next** button:
- Select **HOL Team Project**;
- Select **HOL** Repository;
- Select **Master** as the default branch;
- Check **Continuous Integration**;
- Change the **Default Agent Queue** to **Hosted VS2017**;
- Click **Create**.

<!--- Image needs to be update to highlight the change on the Default Agent Queue to **Hosted VS2017**.
![](<media/CI4.png>)
--->

> **Note:** We may have multiple repositories and branches, so we need to select the correct Repo and Branch before we can select which Solution to build.

**6.** After clicking the **Create** button, On the **Build** tab click the **Add build step** in the Build pane.

![](<media/CI5.png>)

**7.** In the **Add tasks** dialog, select the **Utility** page and then add a **PowerShell** task.

![](<media/CI6.png>)

**8.** Still in the **Add tasks** dialog, select the **Test** page and add a **Publish Test Results**.

 ![](<media/CI7.png>)

**9.** Select the **Utility** page again and add a **Copy and Publish Artifacts** task and then click on **Close**.

![](<media/CI8.png>)

**10.** On the **PowerShell Script** task, click on the blue **rename** pencil icon and change the name of the step to **dotnet restore, build, test and publish** and click **OK**

**11.** Configure the Task:
- Select **File Path** for the **Type** property
- Enter **"build.ps1"** for the **Script filename** property
- Set **$(BuildConfiguration) $(System.DefaultWorkingDirectory) $(build.stagingDirectory)** for the **Arguments** property. 
- Click on Advanced and change the **Working Folder** to **$(System.DefaultWorkingDirectory)**

![](<media/CI9.1.png>)

<!--- Image needs to be updated to have the new parameters as argument.
 ![](<media/CI9.2.png>) 
--->

> **Note:** The build.ps1 script contains commands using the **dotnet.exe** executable used by .Net Core.  The build script does the following: restore, build, test, publish, and produce an MSDeploy zip package.

**12.** On the **Publish Test Results** task make the following changes:
- Change the **Test Result Format** to **VSTest**.
- On the **Test Results File** to **\*\*/testresults.xml**.
- On **Search folder** enter the value **$(System.DefaultWorkingDirectory)**

<!--- Image needs update to show the correct Test Result Format option
![](<media/CI10.png>)
--->

**13.** On the **Copy Publish Artifact** task, change the **Copy Root** property to **$(build.stagingDirectory)**, The **Contents** property to **\*\*\\\*.zip**, The **Artifact Name** property to **drop** and the **Artifact Type** to **Server**.

![](<media/CI11.png>)

**14** Select the **Variables** page and a new variable that will be used by the build.ps1 PowerShell script; **BuildConfiguration** with a value of **release**.

![](<media/CI12.png>)

**15.** Click on the **Triggers** tab and verify that the **Continuous integration (CI)** option is selected to build the solution every time a change is checked in. Also make sure the filter includes the appropriate branch (in this case **master** and **Batch Changes** checkbox is unchecked

![](<media/CI13.png>)

> **Note:** To enable Continuous integration in your project, check the **Continuous integration (CI)** checkbox. You can select which branch you wish to monitor, as well.

**16.** Click **Save** and give the build definition a name.

![](<media/CI14.png>)

### III. Test the CI Trigger in Visual Studio Team Services

We will now test the **Continuous Integration build (CI)** build we created by changing code in the Parts Unlimited project with Visual Studio Team Services.

**1.** Select the **Code** hub and then select your your repo, **HOL**.

**2.** Navigate to **/src/PartsUnlimitedWebsite/Controllers** in the PartsUnlimited project, then click on the ellipsis to the right of **HomeController.cs** and click **Edit**.

![](<media/CI15.png>)

**2.** After clicking **Edit**, add in text (i.e. *This is a test of CI*) after the last *Using* statement. Once complete, click **Save**.

![](<media/CI16.png>)

**3.** Click **Build** hub. This should have triggered the build we previously created.

![](<media/CI17.png>)

**4.** Click on the **Build Number**, and you should get the build in progress. Here you can also see the commands being logged to console and the current steps that the build is on.
![](<media/CI17.1.png>)

**4.** Click on the **Build Number** on the top left and you should get a build summary similar to this, which includes test results.

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
