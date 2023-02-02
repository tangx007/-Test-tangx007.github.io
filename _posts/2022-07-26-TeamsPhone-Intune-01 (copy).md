# How to check the reason why Teams phone didn't meet compliance requirements under MEM/Intune



1) Make Sure your login account is assigned MS Intune License.
2) Follow the below steps under MEM

Device Compliance 

![image-20220726103633385](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726103633385.png)

Select the relevant policy for the Android device, Eg as below

![image-20220726103706352](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726103706352.png)

Under the compliance policy, select Device status

![image-20220726103735459](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726103735459.png)

Now, you can see there is a device not compliance. Then click into to see detail.

![image-20220726104522634](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726104522634.png)

Click Device compliance --> click the not compliance item. It should relate to the Android phone device.

![image-20220726104738066](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726104738066.png)

Eg, as below picture, Mine is not compliance w/ the Min OS Version.

![image-20220726104809758](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220726104809758.png)



由于打开了这个开关

![image-20220809110936363](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809110936363.png)

会导致没有分配合规策略的的设备变成“不合规”，从而无法通过Conditional access policy

![image-20220809111015585](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809111015585.png)

什么原因导致设备没有分配合规策略呢？需要看看在合规策略里面的分配规则，例如现在包含了All users 但排除所有的奥科品牌的电话机

![image-20220809112420209](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809112420209.png)



![image-20220809113044415](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809113044415.png)

![image-20220809113125730](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809113125730.png)

![image-20220809113229895](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809113229895.png)

![image-20220809113252098](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809113252098.png)

![image-20220809113312631](C:\Users\nemot\AppData\Roaming\Typora\typora-user-images\image-20220809113312631.png)





