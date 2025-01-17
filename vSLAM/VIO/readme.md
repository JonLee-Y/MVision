# 视觉惯性里程计 VIO - Visual Inertial Odometry 视觉−惯性导航融合SLAM方案

![](https://pic2.zhimg.com/v2-18cb9f97ac7f759a128b789b681c534f_1200x500.jpg)


[视觉惯性单目SLAM知识 ](https://blog.csdn.net/myarrow/article/details/54694472)

      IO和之前的几种SLAM最大的不同在于两点：
        首先，VIO在硬件上需要传感器的融合，包括相机和六轴陀螺仪，
             相机产生图片，
             六轴陀螺仪产生加速度和角速度。
             相机相对准但相对慢，六轴陀螺仪的原始加速度如果拿来直接积分会在很短的时间飘走（zero-drift），
             但六轴陀螺仪的频率很高，在手机上都有200Hz。
        其次，VIO实现的是一种比较复杂而有效的卡尔曼滤波，比如MSCKF（Multi-State-Constraint-Kalman-Filter），
             侧重的是快速的姿态跟踪，而不花精力来维护全局地图，
             也不做keyframe based SLAM里面的针对地图的全局优化（bundle adjustment）。
             最著名的商业化实现就是Google的Project Tango和已经被苹果收购的Flyby Media，

      其中第二代Project Tango搭载了Nividia TK1并有主动光源的深度摄像头的平板电脑，
      这款硬件可谓每个做算法的小伙伴的梦幻搭档，具体在这里不多阐述。

# 多传感器融合

      传感器融合是一个趋势，也或者说是一个妥协的结果。
      为什么说是妥协呢？
      主要的原因还是由于单一的传感器不能适用所有的场景，
      所以我们寄希望于通过多个传感器的融合达到理想的定位效果。

      1、简单的，目前行业中有很多视觉+IMU的融合方案，
            视觉传感器在大多数纹理丰富的场景中效果很好，
                 但是如果遇到玻璃，白墙等特征较少的场景，基本上无法工作；
            IMU长时间使用有非常大的累积误差，但是在短时间内，其相对位移数据又有很高的精度，
                 所以当视觉传感器失效时，融合IMU数据，能够提高定位的精度。

      2、再比如，无人车当中通常使用 差分GPS + IMU + 激光雷达（视觉）的定位方案。
            差分GPS在天气较好、遮挡较少的情况下能够获得很好的定位精度，
               但是在城市高楼区域、恶劣天气情况下效果下降非常多，
            这时候融合IMU+激光雷达（视觉）的方案刚好能够填补不足。
            
# 惯性传感器（IMU）
      能够测量传感器本体的角速度和加速度，被认为与相机传感器具有明显的互补性，
      而且十分有潜力在融合之后得到更完善的SLAM系统。
      
      1、IMU虽然可以测得角速度和加速度，但这些量都存在明显的漂移（Drift），
         使得积分两次得到的位姿数据非常不可靠。
          好比说，我们将IMU放在桌上不动，用它的读数积分得到的位姿也会漂出十万八千里。
          但是，对于短时间内的快速运动，IMU能够提供一些较好的估计。
          这正是相机的弱点。
          当运动过快时，（卷帘快门的）相机会出现运动模糊，
          或者两帧之间重叠区域太少以至于无法进行特征匹配，
          所以纯视觉SLAM非常害怕快速的运动。
          而有了IMU，即使在相机数据无效的那段时间内，
          我们也能保持一个较好的位姿估计，这是纯视觉SLAM无法做到的。
          
      2、相比于IMU，相机数据基本不会有漂移。
         如果相机放在原地固定不动，那么（在静态场景下）视觉SLAM的位姿估计也是固定不动的。
         所以，
         相机数据可以有效地估计并修正IMU读数中的漂移，使得在慢速运动后的位姿估计依然有效。
         
      3、当图像发生变化时，本质上我们没法知道是相机自身发生了运动，
         还是外界条件发生了变化，所以纯视觉SLAM难以处理动态的障碍物。
         而IMU能够感受到自己的运动信息，从某种程度上减轻动态物体的影响。  
# 难点 
      复杂性主要来源于 IMU测量 加速度 和 角速度 这两个量的事实，所以不得不引入运动学计算。
      
      目前VIO的框架已经定型为两大类：
      按照是否把图像特征信息加入状态向量来进行分类
      1、松耦合（Loosely Coupled）
          松耦合是指IMU和相机分别进行自身的运动估计，然后对其位姿估计结果进行融合。
      2、紧耦合（Tightly Coupled）
          紧耦合是指把IMU的状态与相机的状态合并在一起，共同构建运动方程和观测方程，然后进行状态估计。
          
     紧耦合理论也必将分为 基于滤波(filter-based) 和 基于优化(optimization-based) 两个方向。
     
     1、在滤波方面，传统的EKF以及改进的MSCKF（Multi-State Constraint KF）都取得了一定的成果，
        研究者对EKF也进行了深入的讨论（例如能观性）；
        
     2、 优化方面亦有相应的方案。
      
      值得一提的是，尽管在纯视觉SLAM中优化方法已经占了主流，
      但在VIO中，由于IMU的数据频率非常高，
      对状态进行优化需要的计算量就更大，因此目前仍处于滤波与优化并存的阶段。
      
      VIO为将来SLAM的小型化与低成本化提供了一个非常有效的方向。
      而且结合稀疏直接法，我们有望在低端硬件上取得良好的SLAM或VO效果，是非常有前景的。
      
# 基于滤波器的紧耦合 Filter-based Tightly Coupled method
      紧耦合需要把图像feature进入到特征向量去，
      因此整个系统状态向量的维数会非常高，因此也就需要很高的计算量。
      比较经典的算法是MSCKF，ROVIO
![](https://images2015.cnblogs.com/blog/823608/201701/823608-20170120211824921-442661944.png)
      
## 紧耦合举例-msckf
      据说这也是谷歌tango里面的算法。
      在传统的EKF-SLAM框架中，特征点的信息会加入到特征向量和协方差矩阵里,
      这种方法的缺点是特征点的信息会给一个初始深度和初始协方差，
      如果不正确的话，极容易导致后面不收敛，出现inconsistent的情况。
      
      Msckf维护一个pose的FIFO，按照时间顺序排列，可以称为滑动窗口，
      一个特征点在滑动窗口的几个位姿都被观察到的话，
      就会在这几个位姿间建立约束，从而进行KF的更新。

## ROVIO，紧耦合，图像patch的稀疏前端(?)，EKF后端
[代码](https://github.com/Ewenwan/rovio)


# 基于滤波器的松耦合 Filter-based Tightly Coupled
      松耦合的方法则简单的多，避免把图像的feature加入状态向量，
      而是把图像当成一个black box,计算vo处理之后才和imu数据进行融合。
      Ethz的Stephen Weiss在这方面做了很多的研究，
      他的ssf和msf都是这方面比较优秀的开源算法，有兴趣的读者可以参考他的博士论文。
![](https://images2015.cnblogs.com/blog/823608/201701/823608-20170120212016937-685009538.png)

## 基于滤波器的松耦合举例-ssf
[代码](https://github.com/Ewenwan/ethzasl_sensor_fusion)
      
      滤波器的状态向量 x 是24维，如下，相较于紧耦合的方法会精简很多。
      Ssf_core主要处理state的数据，里面有预测和更新两个过程。
      Ssf_update则处理另外一个传感器的数据，主要完成测量的过程
![](https://images2015.cnblogs.com/blog/823608/201701/823608-20170120212030437-1010714101.png)

## 基于滤波器的松耦合举例-msf
[代码](https://github.com/Ewenwan/ethzasl_msf)

![](http://www.liuxiao.org/wp-content/uploads/2016/07/framesetup-300x144.png)

# 基于优化的松耦合
      随着研究的不断进步和计算平台性能的不断提升，
      optimization-based的方法在slam得到应用，
      很快也就在VIO中得到应用，紧耦合中比较经典的是okvis，松耦合的工作不多。
      大佬Gabe Sibley在iros2016的一篇文章
      《Inertial Aided Dense & Semi-Dense Methods for Robust Direct Visual Odometry》提到了这个方法。
      简单来说就是把vo计算产生的位姿变换添加到imu的优化框架里面去。
      
# 基于优化的 紧耦合 
      
## 基于优化的紧耦合举例-okvis   多目+IMU   使用了ceres solver的优化库。
[代码](https://github.com/Ewenwan/okvis)

![](https://images2015.cnblogs.com/blog/823608/201701/823608-20170120212125265-76552078.png)

      上图左边是纯视觉的odemorty,右边是视觉IMU融合的odemorty结构， 
      这个核心在于Frame通过IMU进行了联合， 
      但是IMU自身测量有一个随机游走的偏置， 
      所以每一次测量又通过这个偏置联合在了一起，
      形成了右边那个结构，对于这个新的结构， 
      我们需要建立一个统一的损失函数进行联合优化.
![](https://pic4.zhimg.com/v2-c00d0a55d9ff7bf23a4ed5249fb1090b_r.png)
      
## 基于优化的 紧耦合 orbslam2 + imu 紧耦合、ORB稀疏前端、图优化后端、带闭环检测和重定位
[代码](https://github.com/Ewenwan/LearnVIORB)

[orb-slam1 + imu](https://github.com/Ewenwan/orb_slam_imu)

## 基于优化的紧耦合  VINS-Mono   港科大的VIO

      前端基于KLT跟踪算法， 后端基于滑动窗口的优化(采用ceres库)， 基于DBoW的回环检测
      
[VINS-Mono  Linux](https://github.com/Ewenwan/VINS-Mono)

[VINS-Mobile MacOS](https://github.com/Ewenwan/VINS-Mobile)

![](https://pic3.zhimg.com/80/v2-145f576a58d1123a9faa1d265af40522_hd.png)


# 2Dlidar（3Dlidar）+IMU

[2Dlidar（3Dlidar）+IMU Google的SLAM cartographer 代码](https://github.com/Ewenwan/cartographer)

