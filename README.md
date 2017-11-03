# PartsUnlimitedDocs-Translation
[PartsUnlimitedDocs](https://github.com/Microsoft/PartsUnlimited/tree/master/docs) Chinese Translation


把 [PartsUnlimitedDocs](https://github.com/Microsoft/PartsUnlimited/tree/master/docs) 这份文档翻译成中文（文档格式为MD）。

这份文档是 微软 [Azure DevOps PartsUnlimited](https://github.com/Microsoft/PartsUnlimited) 的练习文档，原版是英文，想练习一下通过Github来翻译MD格式的文档，所有创建了这份文档。同时在转换、分割整理为公司内部文档（使用TFS）


翻译工具：OmegaT（除了这种客户端工具应该还有在线众译之类的工具吧？没有研究过，未知），如果你有兴趣请联系我 。关于 OmegaT：

- [OmegaT 是一个使用Java编程语言编写的计算机辅助翻译工具,CAT](http://www.cnblogs.com/helinzi/p/4443587.html)
- [OmegaT_3.6.0_07_Windows_without_JRE 下载](https://nchc.dl.sourceforge.net/project/omegat/OmegaT%20-%20Standard/OmegaT%203.6.0%20update%207/OmegaT_3.6.0_07_Windows_without_JRE.exe)
- [http://www.360doc.com/content/15/0805/10/13924396_489618270.shtml](http://www.360doc.com/content/15/0805/10/13924396_489618270.shtml)
- [如何使用OmegaT翻译PureBasic文档](http://v.youku.com/v_show/id_XMTY1ODczMDc2OA==.html?spm=a2h0k.8191407.0.0&from=s1.8-1-1.2)
- [如何使用OmegaT、git实现多人协同翻译上](http://v.youku.com/v_show/id_XMTMxNjUxNTI2OA==.html?from=y1.2-1-87.4.1-1.1-1-2-0-0%26source%3Dautoclick)

关于目录结构，都是在OmegaT中配置好的，不要随意更改目录名和文件名：
- OmegaT：OmegaT 的项目文件，包括词典、词汇等
- Translate： 翻译后的文档，在源文件名的基础上加上了后缀：-CN。此目录中的文件是通过OmegaT自动生成的，文件名也是自动加上了-CN。实际上可以通过他翻译各种语言了
- docs：源文档

第一阶段翻译内容（基于Windows环境）：
- HOL-Continuous_Integration 
- HOL-Continuous_Deployment
- GettingStarted.md

问题：
- OmegaT 机器翻译无法使用，还需研究如何配置
- OmegaT 字典配置

## 于 2017-11-3 周五下午