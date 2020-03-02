# GAU打卡群细则

说明：本Repo为GAU日打卡群仓库，GAU周打卡群仓库请[点击](https://github.com/GAUnion/LeetCode-Weekly)

## 正文

1. 出题：每日北京时间晚上8点，由成员轮流发布leetcode的随机题号。出题成员以及题解成员安排具体详见群内文档，每天出的题目尽量是同一tag题，在发布题号前请先与仓库中的题解核对，防止出现重复题目。同时难度设置方面希望尽可能的每周的分布（从周日开始）满足同一条件，暂定为2hard，7medium，5easy。
2. 打卡：在出题后，从当日晚上8点到次日晚上8点，请各位在LC平台上独立完成一遍，并在群里提交完整的代码截图。
3. 题解：次日晚上8点到12点，需要由指定成员发布前一日的题目的题解并上传本仓库题解文件夹下，要求题解包含英文书写的解题思路和代码（建议两种方法及以上），必要时可以配图。格式需要为markdown格式。 题解标题请注意规范，方便之后检索查看，格式为“数字题号-英文题目名称-难易程度-题目布置时间”，如在3月2号布置了第一题“Two Sum”，则上传的题解名称应为“0001-Two Sum-Easy-03.02.md”。
4. 查验：本群会对布置题目和发布题解进行查验，但暂时不会对打卡进行查验，如果出现超时或者遗忘的情况，会视情况进行提醒或警告，如果态度较为恶劣会被请出群。
5. 本群退群自由，但是同一人退群之后再申请入群的，需要交纳一次性的红包。
6. 有任何问题请在群里提出，大家会帮你解答的！
7. 本群不欢迎任何无意义的喊弱、吹捧、讥讽和贩卖焦虑。

PS：上述部分规则参考[严酷打卡群](<https://wisdompeak.github.io/lc-score-board/rules.html>)

## Q&A

### 如何上传题解到github？

1. 在注册好账户，准备第一次上传前，请先联系群内Jason并告知github账户，会将你设为仓库的contributor，方便进行上传（另外一种方式是走fork + pull request那一套，这个相对来说比较繁琐）。

2. 安装git及后续相关操作（如之前已使用过git可跳过这不） [github教程一](<https://blog.csdn.net/qq_35246620/article/details/66973794>) [github教程二](<https://www.runoob.com/w3cnote/git-guide.html>)

3. 负责代码到本地：由于仓库已经建立好，所以直接clone到本地即可
      
   `git clone https://github.com/GAUnion/LeetCode-Daily.git` 

4. 每次上传前请先更新一下本地仓库 
  
   `git pull` 

5. 编写题解或者将写好的文件移入到“题解”文件夹中

6. 写好后添加文件到缓存区准备提交
  
   `git add .`

7. 使用如下命令以实际提交改动 双引号中的内容可以修改

   `git commit -m "代码提交信息"`

8. 执行如下命令以将这些改动提交到远端仓库

   `git push"`

至此文件成功上传到仓库中，后续还想提交时只需要重复4-8步骤即可。

## 关于GAU

全称Go Abroad Union

![image-20200302211704348](https://github.com/GAUnion/LeetCode-Daily/blob/master/images/gau-1.png)

交大校友 自发创立

公益组织 完全免费

十年积淀 别具一格

文书互改 成员互助

人脉充足 资源丰富

慷慨分享 一同出国

我为人人 人人为我

![image-20200302211704348](https://github.com/GAUnion/LeetCode-Daily/blob/master/images/gau.png)