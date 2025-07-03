# Star image simulation and identification 星图 模拟&识别 
	
*	这个Matlab App是我近两年的工作汇总，为了方便自己的工作，也分享给你
*	本软件所采取的星图识别算法是我2024.2.29发表在IEEE Sensors Journal的工作: [A Star Identification Graph Algorithm Based on Angular Distance Matching Score Transfer](https://doi.org/10.1109/JSEN.2024.3350089)，它在多假星干扰、极端少星(3-4颗)、强噪声条件下的性能优越，如果你有兴趣，欢迎看看文章
*	本软件所采取的PSF估计和空间点目标图像增强算法是我2025.5.23发表在IEEE TIM的工作: [Real-Time Detection, Tracking, and Anomaly Analysis of Dim Small Space Targets for Space Situational Awareness Using Star Sensors](https://ieeexplore.ieee.org/document/11014542)，它通过图像中较亮恒星的高置信度质心、星等信息，联立求解PSF，并设计空间滤波和形态学操作对点光源目标进行增强，同时文章介绍了空间目标的时序跟踪和分析，如果你有兴趣，欢迎看看文章


 这是一个matlab app，您可以用Star_Simu&Iden.mlappinstall安装Matlab APP；
	

![image](https://github.com/user-attachments/assets/d74be242-e239-4269-b0cb-585f75650fbe)


# 软件功能：

1.星图模拟：可以输入姿态参数、相机参数模拟静态或动态星图，可以添加静态或动态的目标(假星)

2.星点提取：从图像中分割星点与背景，提取星点连通域，计算星点的质心坐标。

3.星图识别：识别图像中恒星，具体参见论文Graph Score Transfer

4.其他功能：导入图像、保存图像、镜像翻转图像、反投影星点、星点掩膜、目标探测。

# 面板介绍：

1. Attitude_Input 姿态输入
	Rand按钮：随机生成一组姿态参数，Ra赤经Dec赤纬对应的方向矢量服从在全天球概率均匀分布，Roll滚转角服从 ( -\pi, \pi ] 均匀分布。
	Ra Dec Roll：模拟星图的姿态参数，可以根据需要的参数手动输入。

2. Star_List_Generation 生成星表
	Get StarList按钮：按照"1."中姿态参数对应的光轴以及"4."中相机参数对应的视场，在星库中搜索视场内的恒星，并显示在位于左上的列表中。
	Catalog：可以选择生成星表、模拟星图、反投影时使用的星库，包含极限星等6.0Mv和7.5Mv的两个星库
	Table1：左上的列表为模拟星图时使用的星列表，包含恒星在星库中的序号，质心坐标和星等信息。

3. Object / FakeStar 目标/假星
	Add：在左下的列表中添加一个目标/假星，并在图中显示。位置随机，等效星等和速度按照"3."中设置的值，增益和曝光时间按照"5."中设置的值。
	Delete：删除所有目标/假星，并在图像中使用背景均值抹去目标/假星。
	Mv Vx Vy：设置待添加目标/假星的等效星等、X方向速度和Y方向速度。
	Table2：左下的列表为图像中添加目标/假星的列表，包含质心坐标、速度和等效星等信息。

4. Camera_Parameter 相机参数设置
	NumX, NumY：像素数 单位为 (pixel)
	CenX, CenY：主点坐标 单位为 (pixel)
	DX, DY：传感器像元尺寸 单位为 (mm)
	Focal：相机焦距 单位为 (mm)
	FOV：图像对角线对应的视场角 单位为 (°)

5. Image_Parameter 图像参数设置
	Mode：可以选择statics静态和Dynamic动态模式，动态模式假设相机匀速直线运动，将根据"5."中的曝光时间和速度生成动态星图。生成动态星图的时间比静态稍长一些。
	Clean按钮：清除UI界面中间的图像。
	Simulation按钮：按照左上侧的恒星列表中质心位置和星等信息，并以"5."中的增益生成恒星图像，恒星的成像点扩散函数遵循高斯函数。并按照"5."中的背景均值和方差生成符合正态分布的背景。Simulation过程只生成星图，与"3."中的目标/假星添加过程互相独立。
	Exposure：曝光时间，单位为 (frame帧)，原理为多张星图叠加
	Vx, Vy：动态模式时 相机运动的速度 单位为 (pixel/frame)
	Gain_star：增益，可以按比例调整所有恒星和目标的亮度，同时被"3."中恒星和"5."中目标/假星使用。
	Avg_back, Std_back：背景的均值和标准差

6. Star_Extraction 星点提取
	Star_Extraction按钮：按照灰度阈值对图像进行二值化分割，按照连通域阈值提取恒星/目标，展示在右上的表格中。
	Gray_thres：灰度阈值，单位为灰度，取值范围1-255
	Pixel_thres：连通域阈值，单位为像素
	Num_Star：星点数量，会按照检测/识别结果自动更新。与右上的列表保持一致

7. Star_Identification 星图识别
	Iden按钮：星图识别，将灰度求和排名前20的星进行识别，恒星结果展示在右上的表格中，并在图像相应的位置作蓝色标记。目标/假星结果展示在右下的表格中，并在图像相应的位置作红色标记。计算姿态展示在"7."中的Ra Dec Roll中。
	Ra Dec Roll：识别结果解算的姿态参数。也作为"8."中Projection反投影的姿态参数，此时可以根据反投影需要的参数手动输入。
	Table3：右下的表格，包含提取/识别的恒星质心坐标、灰度和、星编号信息
	
8. Other_Functions 其他功能
	Import Image按钮：导入图像，若需要识别指定星图，可以通过该功能按钮选择星图文件，可以是模拟星图或真实拍摄的星图。导入星图后"4."中的图像尺寸会自动更新，主点位置默认为图像中心，若有高精度标定结果，可以修改。若希望星图识别成功，需要准确设置"4."中的像元尺寸和相机焦距。
	Save Image按钮：保存图像，可以将UI界面中间展示的图像保存至指定路径，保存结果不包含图窗上的标记。
	
	Flip_UpDown按钮 ：上下翻转图像
	Flip_LiftRight按钮 ：左右翻转图像

	Projection按钮：反投影，根据"7."中的姿态参数和"4."中的相机参数，搜索星库中位于视场内的星，更新在右上的表格中，并在图像相应的位置作绿色标记
	Mask按钮：按照右上列表中的恒星质心坐标和预先设定的大小，对恒星进行掩膜处理。

9. Object/FakeStar Detection 目标/假星探测
	Detect按钮：目标检测，检测结果展示在右下的表格中，并在图中红色标记。
	Gray_thres：灰度阈值，单位为灰度，取值范围1-255
	Pixel_thres：连通域阈值，单位为像素
	Num：目标数量，会按照检测/识别结果自动更新。

	* Todo：仅通过阈值分割方法检测能力弱，后续需要考虑更有效的检测方法。
	* Solution：对空间中的点目标（恒星、人造卫星、空间碎片、行星等）的检测，我们有一些新的进展：[Real-Time Detection, Tracking, and Anomaly Analysis of Dim Small Space Targets for Space Situational Awareness Using Star Sensors](https://ieeexplore.ieee.org/document/11014542) 。图像检测方面：通过将恒星的识别结果所包含的星等信息带入成像模型，让所有恒星像素灰度联立方程组，提升点目标成像（点扩散函数）的参数求解精度，利用成像参数提升点目标信噪比。序列检测方面：通过多级假设检验实时增加/更新/删除视场内的目标，基于卡尔曼滤波对目标下一帧位置进行预测，将预测值和观测值在长时段视角跟踪目标，在短时段视角分析目标状态。（会在有空的时候将该功能集成进APP）

* 2.8版本更新（20240316）：
	增加了标定内参功能
 
* 2.9版本更新（20240614）：
	增加7.5星库识别功能

* 3.0版本更新（20240718）：
	重写了后端GST识别算法，美化了界面

* 4.0版本更新（20241205）：
  
	增加了默认保存地址为matlab当前路径下的'StarSimuIdenAPP_save/'文件夹，也可以在function中点击Set SavePath来修改保存路径；

	增加了多帧星图序列模拟功能，在5.Image_Parameter中Mode选择SeqCont连续序列，星点按照速度vx vy设定；SeqRand 随机序列，每一帧的姿态都是随机的；星图序列模拟不会显示所有帧图像，而是会在存储路径下创建一个子文件夹并保存图像。

	修改了曝光时间设定为"ExpoTime / SeqNum"，Statics静态模式下无意义，Dynamic动态模式下为曝光时间，Seq模式下为序列图像帧数。

* 5.0版本更新（20250703）:
  
	增加了最新的研究成果中利用恒星估计成像系统PSF和对暗弱空间目标的信噪比增强算法

	模拟/导入图像后，点击PSF_Estimate即可完成对PSF的计算，显示在Sgm_PSF，同时位于7.Star_Ident中的姿态也会更新（PS：如果姿态有误，则估计的PSF也不可信）

	可以手动输入或估计Sgm_PSF，利用该数值对图像中的暗弱点目标完成增强处理，处理后可以按照正常的提取/识别流程运行程序。

# Papers

When referencing StarImage_SimuIden, please cite these papers:

* [A Star Identification Graph Algorithm Based on Angular Distance Matching Score Transfer](https://ieeexplore.ieee.org/abstract/document/10397074)
```
@ARTICLE{10397074,
  author={Wei, Yuheng and Wei, Xinguo and Liu, Hao and Li, Jian},
  journal={IEEE Sensors Journal}, 
  title={A Star Identification Graph Algorithm Based on Angular Distance Matching Score Transfer}, 
  year={2024},
  volume={24},
  number={5},
  pages={6539-6547},
  keywords={Sensors;Pattern matching;Robustness;Position measurement;Symbols;Satellites;Graphical models;Algorithm design and analysis;Angular distance matching;graph representation algorithm;star identification;star sensor},
  doi={10.1109/JSEN.2024.3350089}}
```
* [Real-Time Detection, Tracking, and Anomaly Analysis of Dim Small Space Targets for Space Situational Awareness Using Star Sensors](https://ieeexplore.ieee.org/document/11014542)
```
@ARTICLE{11014542,
  author={Wei, Yuheng and Chen, Qian and Wei, Xinguo},
  journal={IEEE Transactions on Instrumentation and Measurement}, 
  title={Real-Time Detection, Tracking, and Anomaly Analysis of Dim Small Space Targets for Space Situational Awareness Using Star Sensors}, 
  year={2025},
  volume={74},
  number={},
  pages={1-12},
  keywords={Stars;Space vehicles;Imaging;Signal to noise ratio;Object detection;Target tracking;Real-time systems;Satellites;Aerospace electronics;Feature extraction;Dim space target photoelectric detection;space situation awareness;space target behavior understanding and prediction;star background processing;star sensors},
  doi={10.1109/TIM.2025.3573015}}
```
