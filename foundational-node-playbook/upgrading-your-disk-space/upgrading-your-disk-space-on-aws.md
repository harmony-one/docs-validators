# Upgrading your Disk Space on AWS

The current recommendation for Disk space for running a Harmony Node is 150 GB.

**Step-by-Step Guide to Increase Disk Space in AWS**

Since the launch of the mainnet, more and more people subscribed to become a node runner. The previously recommended 100 GB disk space is not enough for the future needs. This brief tutorial provides a step-by-step instruction on how to increase the disk space of an EC2 instance in AWS.

**Step 1: After logging in to your AWS console, find the EC2 instance running the Harmony node.**

![](https://lh4.googleusercontent.com/58XLfgxDtFhqP1XpY9As-tDYsgkR0zIDG_mT-Dwwh4Df8z4SDeyY86-VUQHyANqLdKe-l8Ns6rmrVP_LonwxVfcVrdEfx-XRPKZegLCP5N8ZFuqQuLWNn3h9aB9cFanPqkLsW0Va)

**Step 2: Click the link \(/dev/xvda\) located in the bottom right area to show the information of the root device.**

![](https://lh3.googleusercontent.com/CqlJwNMlZSUp56R5ULMy81HiLGuDqyHcSUXFUJfqUuD4fDo3BRIxg5qJvsUje6r7eLJ15Aq0nRIsTJ7UadZjW8p85W_HRh_BDT0A0FelYqhjj7lbqN06pDRxqIRSauQkVbLZDwNd)

**Step 3: Click the EBS ID link \(vol-05685721145114c52 for this demo, your volume might have a different identifier\), this will jump to a web page to modify volume size.**

![](https://lh5.googleusercontent.com/W7mcxaeHXOwwHJJuXT_SkNGxVqFJx9YOlri2FQuIRYcAPBbaIunLheMDNadKRmyB9WyTEk0cJkWKGylJ2DI_i0xSgFsKTcN7rhFRVOsUK_HqX8RmP0qSdXlya27FGNrpLMfuxeQj)

**Step 4: Click the Actions drop down button, and then select “Modify Volume” option. We suggest extending to the size to 150 GiB, but we use 60 GiB for this demo. Then click “Modify”.**

![](https://lh3.googleusercontent.com/h2TXgnhPOK0QJ0QK_oJaxZVrpjgLpbogLhCss0G94NZ5pxz6CHKqWZ9aq_bTwhMqfvXpK4Z6kNmS4QYDbmGw40urdg57dR2jiON5c4Z6Ve3IgdzftkrU3i7AtKdjoR3mGdyy7u_0)

**Step 5: Click “Yes” to confirm this action.**

![](https://lh3.googleusercontent.com/LftO5xVB2P1WtLtAvb72A5eHnqtPJxhAiPOZKXA4gRkLzWCg01E552VptEWQX5yoqPhFzTdnIsuSiSsGMakEsVYbaVz5lp3FgzDPWhK3-uFUlJ6WopKK8M9aEvhDTeD2vZBtOyL2)

**Step 6: it may take 20 to 30 mins to resize the disk space. The action is done until the state change from “in-use modifying…” yellow circle to “in use” green circle.**

**Step 7: Then the next step is to ssh to the node. Check the basic information about the block devices.**

| **lsblk** |
| :--- |


![](https://lh5.googleusercontent.com/ODMMcj-NpcFrjGU0zPVqnnGI8Z3hlAAuqKudZ-rTMEgk_WEIfFzTVOVYbtRbUhwoED6YkZyF-Ppj3m6EVqtp5pj7SqzoULAD82kY-cNWc7AJbHNXab4ZVCw9c4kVMj9VlHn6XTG9)

| **sudo growpart /dev/nvme0n1 1** |
| :--- |


**Expected result:**

![](https://lh5.googleusercontent.com/x7TIcEZaAGv5XJCZVvDKhgIn75IMwYNq4yCKbgAjtqN7WXCl6mIH7xq951BQeRzxSvU6vCZVByWqbJHl4KaXD7JvYm3s3jJrW8XpmewqKQR44IUKomSLcldrSZNTfNtFnHQAaS_z)

| **sudo xfs\_growfs /dev/nvme0n1p1** |
| :--- |


**Expected result:**

![](https://lh5.googleusercontent.com/ieCxsL4-WnvcqkD5PH8AbkkQgcD1dKzMOrbwv2KUP8AfwWZUCgRT8FQHmazY3yI121HWJEv1L-shh52iRz-pEyo8g0SNHkkRQ5afLG3BGOjK1SCc5Di798OjGRRYUQf8iv_Y3L8u)

**Step 8: verification - if you see the size of /dev/nvme0n1p1 has been increased to the specified size \(60G in our demo\), you are good to go.**

| **df -h** |
| :--- |


![](https://lh4.googleusercontent.com/fwtSxCiziGBEUXqNZB42Pe_3FsuH42wSwzbTkcouZogTo2DfSHgrdzb7SvA55PT4FpqK3-Z3fL3AwG4vc9mInbIP9IbIzpdD-twntessQYKN-ohz0HhiTy8O8hTEIXiICKYDEQtB)

**In case you need some extra help for this task, feel free to reach out to our engineer \(Andy@harmony.one\) on Discord, Wechat, and email.**

