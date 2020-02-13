# 动态扩展AWS云服务器硬盘空间

**自从我们Harmony启动了主网以来，越来越多的投资者表示了强烈的兴趣来运行节点。随着加入主网的节点数目增加，我们以前推荐的30G的硬盘空间已经有些捉襟见肘了。这份简短的文档告诉大家怎样手把手的来动态扩展AWS云服务器硬盘空间。**

**第一步：登录AWS控制面板，找到运行节点的服务器。**

![](https://lh4.googleusercontent.com/AnbEsHkxKv99_o2AkoHNfPB-r7MiDS4BwcUjnwIE-6xjJ8MnaVeLePn-CPkf1TVPvKsU-FKuFr72Z5_BSdpSE6ipNr1yjFWVfwXhy49ilq-hfnfz0QINqDB00TtrSaRrq2ErFShN)

**第二步：点击位于屏幕右下区域的链接\(/dev/xvda\)，浏览器会显示硬盘的具体信息。**

![](https://lh3.googleusercontent.com/3LJ590TR-eUKr_NTp3cfbhcBrYqbJmOUKa-ZtHgeYiZqdOEf55dUij1rtQNF_hp0rT8sX7G7NHhpQuYNof0VAzew00XjdSTw9H4kGVvBs4HUMQ9q7lM0X8HsRZ0bz8sJdmnmWaCA)

**第三步：点击 EBS ID 右边的链接（在我们的演示是 vol-05685721145114c52, 你的硬盘可能显示的是不同的名字），然后会跳转到硬盘的显示页面。**

![](https://lh6.googleusercontent.com/0-Bf9bqdPcGryJkz7QmonKhwSFpZZtdYcZbSu9hwbgpr0FZEqEi8wNm1pzlI10uAUw9djB7ESsfUtNFrTLbDdGKmnUZ3YvAc_kWpOzUml1wm0RTrOXAWwFjRX-EujstZ0zjwurWh)

**第四步：点击“Actions”按钮，然后选择“Modify Volume”选项。我们推荐扩展硬盘到150G，但是在这个演示里，我们把硬盘空间扩展到60G。点击“Modify”。**

![](https://lh6.googleusercontent.com/nUg1WIOCmQdhAUTVRYYneGpH0z1E_zvbAWJm0G5QAwGXhQ1PuccvcST_BdyIXPUhnuNV7Npyrvt5w9qwUweQRfAGS3EQDi67rt-SQN1ydZJd9koCXsxuIR1iLRSYlwJB_kIT98QQ)

**第五步：点击“Yes”确认扩展硬盘空间。**

![](https://lh6.googleusercontent.com/1KEEF60YnQH5UJXBxv62yV1mAGqax1UnVE6uU1SWHjajgMGmFos8z_iQVBwWUnxpzqUwPzWyO7OP9Y_m-JSNO8ECfNIN4YS_ELQzYWLjS6_LxdeAMtLM846dvZm4vmeiNmImPd80)

**第六步：硬盘扩容可能需要20到30分钟的时间。当扩容结束以后，你会看到状态栏会从黄色的圆球变成绿色的圆球。**

**第七步：SSH登录到服务器，查看磁盘基本信息。**

**运行以下命令**

| **$lsblk** |
| :--- |


**预期的结果**

![](https://lh3.googleusercontent.com/uj1QDvg9FXEn8sPrtGXsJ7CTt9txntVJ-lnD3uZdPizoYYLQSoE6IAoUG6rBu2aeKsRlreWY9QVPuU39OB3UYz9ioeJS0qThOTHTF_on5XQgIpkau6n4QhC9T5dgdnFcsXPhJhqR)

**运行以下命令**

| **$sudo growpart /dev/nvme0n1 1** |
| :--- |


**预期的结果**

![](https://lh5.googleusercontent.com/FXqYoUTDUloMswBhTTutjj_VYbsDaUZXjiUTpru-82yybK7yhWCEI0WsDv8s0JJuttcDP53TDHbyM6Vou9aKmBoJGGEGNjOKJMzfNf8OIKDK9qrn_h47cVJ9Qg1Wk6xhA8hZuGoH)

**运行以下命令**

| **$sudo xfs\_growfs /dev/nvme0n1p1** |
| :--- |


**预期的结果**

![](https://lh3.googleusercontent.com/43mo4W5GIEGMt_MZH7dZiD08Crx4KOySrYllt2OVgdxelxzeRrDkWF3Co3WlGOBzwllTSXWNZgdAaR8XStVdDtROvW9gAd78ZJbWmITP2ZYyFCBvB-YclARw0Qx6MeRTIOwtDOxI)

**第八步：结果确认。运行以下命令，如果你看到/dev/nvme0n1p1的大小已经从30G变成了60G，恭喜你！AWS云服务器硬盘空间成功！**

**运行以下命令**

| **$df -h** |
| :--- |


**预期的结果**

![](https://lh3.googleusercontent.com/r1L1tiQscv23ow1s6fwRvuOetGhRYlgLnIMGf_x3FQuGhd07ahkTjQ-OuCgXhCbYJF3ZC21WQ17nhxy7YKk1PklR8nGxKXAlTvhMnK0kYZ00nE2tFqOx2lT89QYEhe4B_WmbbbxL)

**如果关于这个项目你需要更多的技术方面支持，可以和我们的技术工程师\(Andy@harmony.one\)联系。你可以在Discord, 或者微信上面找到他。**

