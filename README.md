# Minecraft-GridMap-Gene

## 工作原理：
  使用游戏mod实现游戏内地图的局部提取，生成文件map.txt ，在ego-planner端加入部分代码，实现map.txt 文件的读取。
  
## 工作环境：
  游戏mod对应 minecraft 1.15.2版本,依赖forge v31.2.0 。推荐使用 HMCL 启动器在Windows系统中安装（Ubuntu 大概只有官方启动器能用，需要正版账号）
  
## 配置步骤：
  ### 1. Minecraft端配置
  以 windows 下 HMCL 启动器为例：
  
  #### 1. 安装JAVA JRE
  直接搜索java jre 或者去java官网寻找下载链接，安装java 运行环境。建议java版本为java 8。如果在cmd下输入 java -version 不报错，说明电脑上已经存在java，则这一步骤可省略。
  安装完成后需要设置一下环境变量。
  
  #### 2. 安装 Minecraft 1.15.2 with forge
  打开HMCL启动器，游戏列表 -> 全局游戏设置， 选择java路径。如果上一步安装正确，且设置好环境变量的话，这里会出现对应的选项。
  ![image](https://user-images.githubusercontent.com/21134117/129296839-af4ab208-cbd2-45e8-ab5a-78723dc70bce.png)
  ![image](https://user-images.githubusercontent.com/21134117/129296956-59dd7e75-8221-4c1c-9f6a-6ab0792445d9.png)
  
  注意一下 __运行路径__ ，稍后安装mod需要用到。
  
  返回主页，点击 账户 ，新建游戏账户，登陆方式选离线，用户名自定义。
  
  点击 游戏列表，安装新游戏版本，安装稳定版 1.15.2 ，要求 forge 选择 31.2.0 ， 点击安装，等待。过程中需要在外网下载一些内容，挂梯子。
  
  安装完毕后，先不要启动，找到 __运行路径__ 下的mods文件夹，将 mapGene.jar 拖入其中。
  
  随后便可以运行游戏。
  
  启动后，点击Mods按钮，若能找到Example Mod，则安装正确。
  ![image](https://user-images.githubusercontent.com/21134117/129301957-e74c03d4-b41e-449b-bf7a-fe942b82a766.png)

 
  在ubuntu下的配置步骤相同，操作有一定区别，我没有在ubuntu实践过，就不做说明了。
  
  ### 2. Ego-planner 的修改
  修改的文件在需要修改4个地方, code中提供了一个修改过的ego-planner可以参考：
  
  #### 1. uav_simulator/mockamap/include/map.hpp:
  在Map类的private成员中新增了一个地图生成函数 (第44行)
  void mcMapGenerate();
  
    
  #### 2. uav_simulator/mockamap/src/map.cpp:
  mcMapGenerate() 函数的实现。第16 ~ 93行实现了mcMapGenerate 函数，即从外部文件读取地图，转化为点云。第797 ~ 800行添加了这个函数的接口。
  
  __注意:第22行文件路径需要自己改一下__
  
  #### 3. uav_simulator/mockamap/launch/mockamap.launch:
  第16行将此参数改为5 ，则使用mcMapGenerate() 为地图生成方式。
  
  #### 4. planner/plan_env/src/grid_map.cpp:
  第757行，将 inf_step_z = 1; 改为 inf_step_z = inf_step; 这样生成的障碍物厚度就正常了。
  
  
## 使用方法：
  
   进入游戏，按t打开聊天框，输入指令 /give @a examplemod:magic_block_item 获得一个magic block
   
   将其放置在要框选部分的体对角线两个端点处，可参考下图，黑紫相间的方块就是放置的magic block
   ![image](https://user-images.githubusercontent.com/21134117/129302134-9c99f3b5-3753-4ba0-9e6e-db6a2a1bea93.png)
    
    
   依次破坏这两个方块，破坏时，聊天框会弹出对应提示，如下图所示：
   ![image](https://user-images.githubusercontent.com/21134117/129302221-11392c51-638c-4861-8bfd-7912ce139a3b.png)
   
   随后，在 __运行路径__ 会多出一个map.txt文件。 将其移动到 ego_planner 目录下（具体 uav_simulator/mockamap/src/map.cpp 第22行指定的位置）
   
   编译，运行 ego-planner , 在rviz中调节点云size 和resolution一致，即可得到对应地图：
   
   ![image](https://user-images.githubusercontent.com/21134117/129302458-63540c58-fbb0-495d-9e11-e9466d2f9922.png)
  
    坐标系关系：minecraft 中的x-y-z 轴，对应rviz 中 x - z - (-y) 
    
    
## 效果参考
    
   ![QQ图片20210826110806](https://user-images.githubusercontent.com/21134117/130894170-bf943773-215a-493f-bcc9-31a2f214b364.jpg)

    
    可配合worldEdit快速搭建几何地图

  
  

  
