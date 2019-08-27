### 利用OtipX实现毫米波的传播预测

#### INTRODUCTION

首先介绍了现如今射线追踪技术被广泛地在GPU上实现。但是起初的一些实现都是针对特定的路径衰减模型，不能在其他的场景下实现。之后有了一些商用软件实现了普适的实现，它里面封装了一个材料参数的库，只要场景的材料参数在库里面有，就能利用这个软件仿真，但是因为是
商用软件，不开源还要收费。于是此文就想要基于电磁波传播的机制来实现不同模型的电波传播预估。考虑到场景建模和场景数据预处理的麻烦，本文就直接使用了现有的测试数据用作仿真和对比，同时还和另外一款叫Paray的软件结果进行对比。

#### 仿真引擎
简单介绍了Paray，详细地介绍了OptiX的机制，比如接收球，三角面判交和镜像法（反向追踪进一步筛选路径）等等。

#### PERFORMANCE & ACCURACY
搭了一个会议室的3D场景，在粗粒度、中等粒度和细粒度的三种情况下进行仿真。将结果与之前的Paray的结果对比，发现在大规模的参数对比上，比如路径损耗，RMS传播时延能够匹配上，而在small-scale-fading characteristics上
不能复现。考虑到应该是仿真是只应用了基本的传播机制向反射透射，而忽略了绕射和散射。接下来就是针对能够体现性能的结果参数，比如路径数和仿真时间等的对比。
##### Scenario with conducting surfaces
设置了最大反射次数（4，6，8），然后发射机接收机是垂直极化各向同性源。
得到结果对比，

![icon](img/fig2.jpg)

![icon](img/fig3.jpg)

![icon](img/table3&4.jpg)

##### Realistic Scenario
此场景设置最大反射次数和最大透射次数,改变了天线的类型。

> 比较的结果参数：averaged power delay profile (APDP) 和RMS delay spread

![icon](img/fig4.jpg)

![icon](img/table5.jpg)

#### 关键点
此文章也多次提出由于未考虑散射和绕射所带来的仿真结果上的误差
### GPU高效地实现高频的SBR-PO算法(后向散射的预测)
#### INTRODUCTION
SBR-PO计算量大，费时。
出现了很多加速SBR-PO的方法，其中包括利用GPU。而本文将复杂的射线追踪计算交给了OptiX。
#### IMPLEMENTATION ON A GPU
介绍了OptiX的机制,SBR中二次电磁场源的问题会带来线程分裂。存储的优化，精简GPU向CPU的输出结果。
#### NUMERICAL RESULTS

两个实验，验证了OPTIX的判交和多反射计算的准确性

#### COMPARISON AGAINST FEKO—A SCALED SHIP MODEL
为了更加深入地验证准确性和效率
对包含14004个三角面地船模型进行仿真

![icon](img/2fig4.jpg)

![icon](img/2fig5.jpg)

![icon](img/2table1.jpg)

> 上面轻微地差异很可能是绕射地效果

对多个电大尺寸地结构进行了仿真

![icon](img/2fig6.jpg)

>  In the current implementation, the CPU only handles I/O, but participates in little computations and is idle most of the time.

### 运用射线追踪构建FMCW雷达系统

#### INTRODUCTION
介绍驾驶辅助系统、自动驾驶对场景感知的依赖性很高，强调了实现虚拟的传感器是如此的重要。

介绍了这种虚拟传感器的底层接口-FMCW雷达系统。而采用向有限元法、FDTD达不到系统的性能要求，最后只剩下几何射线法。

介绍ray launching 介绍了OptiX的机制
#### Modeling
ray tube "compact" ray tube
> When calculating
the interaction of a ray tube with a surface, only the orientation of the hit point of the ray on
the surface is regarded

##### Antenna Transmission

半球发射射线管

![icon](img/3fig1.jpg)

##### Reflection and Divergence
射线管达到物体之后生成的新的射线管(有点不懂)

##### Antenna Reception

double counting can be neglected

##### Radar front end

#### Implementation and Results
> an implementation of the model using Nvidia OptiX integrated in the
driving simulator Virtual Test Drive (VTD)

> transmit 50 000 initial rays at a frame rate of 10Hz on a standard workstation in a motorway
scene. The scene consists
of six vehicles, guardrails, vegetation and traffic signs leading to 326 831 triangles

#### 验证
> a 77GHz FMCW debug radar (Radarbook from INRAS) with access
to the A/D converter data is used.

![icon](img/3fig2.jpg)
