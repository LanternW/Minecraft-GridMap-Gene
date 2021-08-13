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
 
  在ubuntu下的配置步骤相同，操作有一定区别，我没有在ubuntu实践过，就不做说明了。
  
  ### 2. Ego-planner 的修改
  修改的文件在需要修改4个地方：
  
  #### 1. uav_simulator/mockamap/include/map.hpp:
  在Map类的private成员中新增了一个地图生成函数 (第44行)
    void mcMapGenerate();
    
  #### 2. uav_simulator/mockamap/src/map.cpp:
  mcMapGenerate() 函数的实现。第16 ~ 73行实现了mcMapGenerate 函数，即从外部文件读取地图，转化为点云。第777 ~ 780行添加了这个函数的接口。
  
  #### 3. uav_simulator/mockamap/launch/mockamap.launch:
  第16行将此参数改为5 ，则使用mcMapGenerate() 为地图生成方式。
  
  #### 4. planner/plan_env/src/grid_map.cpp:
  第757行，将 inf_step_z = 1; 改为 inf_step_z = inf_step; 这样生成的障碍物厚度就正常了。
  
  
## 使用方法：
  

  
