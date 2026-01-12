This package consists of Matlab codes set_channel_params.m and channel_simulator.m and 
a series of Matlab functions that will be called by the main codes. 

To change the channel parameters, edit set_channel_params.m and compile to generate the
[file_name].prm and [file_name].dop data files. Alternatively, you can directly 
update the data files via a text editing software. 
Once a set of data files are generated, run channel_simulator.m to simulate the channel 
for [file_name].prm and [file_name].dop.
 
The simulator takes into account both large- and small-scale variations as well as motion-
induced Doppler. For more information on the channel model, 
please visit http://millitsa.coe.neu.edu/?q=research/simulator

For more information about the Takagi factorization method and the enclosed implementation
package, please visit http://www.cas.mcmaster.ca/~qiao/software/takagi/matlab/readme.html

该软件包包含 MATLAB 主程序 set_channel_params.m 和 channel_simulator.m，以及一系列将由主程序调用的 MATLAB 函数。

如需修改信道参数，请编辑 set_channel_params.m，并进行编译/运行以生成 [file_name].prm 和 [file_name].dop 两个数据文件。或者，你也可以使用文本编辑软件直接更新这些数据文件。
当生成好一组数据文件后，运行 channel_simulator.m，即可基于 [file_name].prm 和 [file_name].dop 对信道进行仿真。

该仿真器同时考虑大尺度与小尺度变化，以及运动引起的多普勒效应。关于信道模型的更多信息，请访问：
http://millitsa.coe.neu.edu/?q=research/simulator

关于 Takagi 分解方法及其随附实现包的更多信息，请访问：
http://www.cas.mcmaster.ca/~qiao/software/takagi/matlab/readme.html

