# 目录

* [一. 试验设计](#一--试验设计)  
* [二. Excel原始数据与SDTM-spec](#二--Excel原始数据与SDTM-spec)   
* [三. SAS操作过程](#三--SAS操作过程)
    * [1. 读入Excel原始数据与SDTM-spec](#1--读入Excel原始数据与SDTM-spec)   
    * [2. DM](#2--DM)   
    * [3. SUPPDM](#3--SUPPDM)  
    * [4. EX](#4--EX)  
    * [5. EF自定义域](#5--EF自定义域)  
    * [6. DS](#6--DS)     
* [四. SDTM结果数据集展示](#四--SDTM结果数据集展示)  
* [五. 宏代码](#五--宏代码)  
    * [1. 宏%getblank](#1--宏%getblank)  
    * [2. 宏%getSEQ](#2--宏%getSEQ)   
    * [3. 宏%DelScreenFailure](#3--宏%DelScreenFailure)  
    * [4. 宏%AddScreenDisposition](#4--宏%AddScreenDisposition)  
 
    
&ensp;&ensp;&ensp;&ensp;  
# 一  试验设计  
&ensp;&ensp;&ensp;&ensp;本临床试验对比研究三种牙膏配方缓解牙龈炎的效果。三组、随机、平行，受试者在家早晚各刷牙一次，测试基线、2周、4周的MBI（平均出血指数）、MGI（平均牙龈指数）、MPI（平均菌斑指数）值。基线当天，受试者先签署知情同意书，测试MBI指标，MBI>1.5的受试者纳入试验，接着检测MGI和MPI。之后，所有被纳入的受试者随机分入三组（Sample A/Sample B/Sample C），受试者从第二天开始早晚使用相应牙膏刷牙，定时返回试验中心检查指标。  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;示意图如下：  
&ensp;&ensp;&ensp;&ensp; 
