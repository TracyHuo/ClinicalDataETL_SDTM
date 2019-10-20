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
  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;    

# 二  Excel原始数据与SDTM-spec  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;本次试验收集了demographics, exposure, disposition, efficacy相关的Excel格式数据，分别是如下的 DM_raw.xlsx, EX_raw.xlsx, DS_raw.xlsx, EF_raw.xlsx，还有随机表RAND.xlsx。另外，本次试验指定了SDTM相关domains的spec，分别为DM domain的DM_map.xlsx，SUPPDM的SUPPDM_map.xlsx，EX domain的EX_map.xlsx，DS domain的DS_map.xlsx，还有自定义域EF domain的EF_map.xlsx展示如下：  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;DM_raw.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;EX_raw.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;EF_raw.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;RAND.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;DM_map.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;SUPPDM_map.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;EX_map.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;DS_map.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;EF_map.xlsx：  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;    

# 三  SAS操作过程  
&ensp;&ensp;&ensp;&ensp;   
## 1  读入Excel原始数据与SDTM-spec  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;ReadExcel.sas  
```
/*******************************************
本文件用来导入以excel格式存储的原始数据和map表
导入DM_raw.xlsx为GINRAW.DM_raw数据集
导入DM_map.xlsx为GINMap.DM_map数据集
导入EX_raw.xlsx为GINRAW.EX_raw数据集
导入EX_map.xlsx为GINMap.EX_map数据集
导入RAND.xlsx为GINRAW.RAND数据集
导入EF_raw.xlsx为GINRAW.EF_raw数据集
导入EF_map.xlsx为GINMap.EF_map数据集
导入DS_raw.xlsx为GINRAW.DS_raw数据集
导入DS_map.xlsx为GINMap.DS_map数据集
导入SUPPDM_map.xlsx为GINMap.SUPPDM_map数据集
 ********************************************/

/*导入excel时，excel文件应处于关闭状态，否则报错*/
PROC IMPORT DATAFILE = "F:\Gingi\GINRaw\DM_raw.xlsx"  
            OUT = GINRAW.DM_raw
            DBMS = xlsx
            REPLACE;
            RANGE = "DM_raw$A1:E33"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINMap\DM_map.xlsx"  
            OUT = GINMAP.DM_map
            DBMS = xlsx
            REPLACE;
            RANGE = "DM_map$A1:L20"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINRaw\EX_raw.xlsx"  
            OUT = GINRAW.EX_raw
            DBMS = xlsx
            REPLACE;
            RANGE = "EX_raw$A1:D31"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINMap\EX_map.xlsx"  
            OUT = GINMAP.EX_map
            DBMS = xlsx
            REPLACE;
            RANGE = "EX_map$A1:L15"; 
            GETNAMES = YES ;
    RUN; 

PROC IMPORT DATAFILE = "F:\Gingi\GINRaw\RAND.xlsx"  
            OUT = GINRAW.RAND
            DBMS = xlsx
            REPLACE;
            RANGE = "RAND$A1:D31"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINRaw\EF_raw.xlsx"  
            OUT = GINRAW.EF_raw
            DBMS = xlsx
            REPLACE;
            RANGE = "EF_raw$A1:M33"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINMap\EF_map.xlsx"  
            OUT = GINMAP.EF_map
            DBMS = xlsx
            REPLACE;
            RANGE = "EF_map$A1:L19"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINRaw\DS_raw.xlsx"  
            OUT = GINRAW.DS_raw
            DBMS = xlsx
            REPLACE;
            RANGE = "DS_raw$A1:E33"; 
            GETNAMES = YES ;
    RUN; 

PROC IMPORT DATAFILE = "F:\Gingi\GINMap\DS_map.xlsx"  
            OUT = GINMAP.DS_map
            DBMS = xlsx
            REPLACE;
            RANGE = "DS_map$A1:L13"; 
            GETNAMES = YES ;
    RUN;

PROC IMPORT DATAFILE = "F:\Gingi\GINMap\SUPPDM_map.xlsx"  
            OUT = GINMAP.SUPPDM_map
            DBMS = xlsx
            REPLACE;
            RANGE = "SUPPDM_map$A1:L11"; 
            GETNAMES = YES ;
    RUN;

```
&ensp;&ensp;&ensp;&ensp;   
## 2  DM  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;GetDM.sas  
```
/***********************************************************
 下方代码用于生成SDTM里的DM数据集。
 ***********************************************************/


/*调用宏%getblank生成 空白数据集 GINMAP.DM_blank 。*/
OPTIONS MSTORED SASMSTORE=GINMacro ;
%getblank(maptable=GINMAP.DM_map , dsout=GINMAP.DM_blank);

/*此DATA步修改原始数据集DM_raw里的基本变量，返回DM_basic数据集。*/
DATA DM_basic;
    SET GINRaw.DM_raw ;
	STUDYID = "1001";
	DOMAIN = "DM";
	SITEID = "01";
	USUBJID = trim(left(STUDYID))||"-"||trim(left(SITEID))||"-"||trim(left(SUBJID)) ;
	COUNTRY = "CHINA";
	AGE = int((input(RFICDAT,yymmdd10.)-input(BRTHDAT,yymmdd10.)+1)/365.25);
	if age ^=. then AGEU = "YEARS"; /*若年龄不为空，才赋单位。因为有可能有没收集到年龄的情况。*/
    IF RACE^="汉族" THEN RACE="OTHER";
    KEEP STUDYID DOMAIN USUBJID SUBJID SITEID AGE AGEU COUNTRY SEX RACE BRTHDAT;
RUN;

/*此步为DM域生成ARM相关变量，返回DM_arm数据集*/
DATA DM_arm;
    MERGE GINRaw.DM_raw(in=a) GINRaw.RAND(in=b) ;
	BY SUBJID;
	IF a=1 AND b=0 THEN DO;
	    ARMNRS="SCREEN FAILURE";
		ARM = "";
		ARMCD = "";
	END;
	KEEP SUBJID ARMCD ARM ARM ARMNRS;
	/*注意，如果受试者是screen failure，则ARM, ARMCD变量都为Null，ARMNRS解释原因*/
RUN;

/*此DATA步修改原始数据集DM_raw里的日期时间变量，返回DM_date数据集。*/
DATA DM_date ;
    MERGE GINRaw.DM_raw(in=a)  GINRaw.EX_raw(in=b);
	BY SUBJID; /*因为本实验中SUBJID已经是唯一的了，所以可以作为合并依赖变量。*/
    
	IF a=1 AND b=1 THEN DO;
	    RFSTDAT = EXSTDAT ; /*创建RFSTDAT变量，来自EX域的EXSTDAT变量，两者都是num型，后序都要转化。*/
	    RFENDAT = EXENDAT ; /*同上*/
	    RFPENDAT = EXENDAT ; /*同上*/
	END;
	ELSE IF a=1 AND b=0 THEN DO;
	    RFPENDAT = RFICDAT ; /*此就是screen failure的情况，此时RFPENDAT有值，为visit 1当天的日期*/
		RFSTDAT = ""; /*此为screen failure的情况，受试者没被treat，所以RFSTDAT值为null*/
		RFENDAT = ""; /*同上*/
	END;

	/*本实验，DM数据收集时间即为visit 1的时间，即签知情同意当天，等于RFICDAT。
	  而DMDY是study day，study day本身以RFSTDAT作为reference。*/
	DMDYDAT = RFICDAT ;
	IF input(DMDYDAT,yymmdd10.)>= input(RFSTDAT,yymmdd10.) THEN 
	    DMDY = input(DMDYDAT,yymmdd10.)- input(RFSTDAT,yymmdd10.)+1 ;
	ELSE DMDY = input(DMDYDAT,yymmdd10.)- input(RFSTDAT,yymmdd10.);

    KEEP SUBJID RFSTDAT RFENDAT RFICDAT RFPENDAT DMDY ;
RUN;

/*此DATA步用于将前边的所有数据集（包括空表）merge到一起，
  然后将--DAT变量赋给ISO 8601格式的--DTC变量，得到最终的SDTM 
  DM表。*/
DATA GINSDTM.DM ;
    MERGE GINMAP.DM_blank DM_basic DM_arm DM_date ;
	BY SUBJID;
    /*因为DM_blank位于最前边，所以决定了所有变量的位置和顺序，而这些信息
	  是直接来自map表的，所以是符合要求的。*/
    RFICDTC = put(input(RFICDAT,yymmdd10.),e8601da10.); /* 可以这样格式转换。但左侧变量必是新建的。*/
    RFSTDTC = put(input(RFSTDAT,yymmdd10.),e8601da10.);
    RFENDTC = put(input(RFENDAT,yymmdd10.),e8601da10.);
    RFPENDTC = put(input(RFPENDAT,yymmdd10.),e8601da10.); 
    BRTHDTC = put(input(BRTHDAT,yymmdd10.),e8601da10.);

    DROP RFICDAT RFSTDAT RFENDAT RFPENDAT BRTHDAT;
RUN;

```
&ensp;&ensp;&ensp;&ensp;   
## 3  SUPPDM  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;GetSUPPDM.sas  
```  
/***********************************************************
 下方代码用于生成SUPPDM数据集。
 ***********************************************************/


/*调用宏%getblank生成 空白数据集 GINMAP.SUPPDM_blank 。*/
OPTIONS MSTORED SASMSTORE=GINMacro ;
%getblank(maptable=GINMAP.SUPPDM_map , dsout=GINMAP.SUPPDM_blank);

/*此DATA步以原始数据集DM_raw为基础，创建SUPPDM数据集，它包含了SUPPDM
  域里应包含的基本信息。*/
DATA SUPPDM ;
    SET GINRAW.DM_raw;
	IF RACE^="汉族";
	STUDYID = "1001";
	RDOMAIN = "DM";
	SITEID = "01";
	USUBJID = trim(left(STUDYID))||"-"||trim(left(SITEID))||"-"||trim(left(SUBJID)) ;
	QNAM="RACEOTH";
	QLABEL="Race, Other";
	QORIG="CRF";
	RENAME RACE=QVAL;
	DROP SITEID RFICDAT BRTHDAT SEX SUBJID;
RUN;

/*将上述数据集与空表merge，得到最终SUPPDM数据集*/
DATA GINSDTM.SUPPDM ;
    MERGE GINMAP.SUPPDM_blank SUPPDM ;
	BY USUBJID;
RUN;


```  
&ensp;&ensp;&ensp;&ensp;   
## 4  EX  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;GetEX.sas  
```  
/***********************************************************
 下方代码用于生成SDTM里的EX数据集。
 ***********************************************************/


/*调用宏%getblank生成 空白数据集 GINMAP.EX_blank 。*/
OPTIONS MSTORED SASMSTORE=GINMacro ;
%getblank(maptable=GINMAP.EX_map , dsout=GINMAP.EX_blank);

/*此DATA步修改原始数据集EX_raw里的基本变量，返回EX_basic数据集。*/
DATA EX_basic;
    SET GINRaw.EX_raw ;
	STUDYID = "1001";
	DOMAIN = "EX";
	SITEID = "01";
	USUBJID = trim(left(STUDYID))||"-"||trim(left(SITEID))||"-"||trim(left(SUBJID)) ;
	EXDOSE = 2 ;
	EXDOSU = "g";
	EXDOSFRQ = "BID";
	EPOCH = "TREATMENT" ;/*整个使用样品的4周阶段是一个epoch，名为TREATMENT*/
	
	DROP SITEID ;
RUN;

/*此DATA步修改原始数据集EX_raw里的日期时间变量，返回EX_date数据集。*/
DATA EX_date ;
    MERGE GINSDTM.DM(keep=SUBJID USUBJID RFSTDTC in=a)  GINRaw.EX_raw(in=b);
	BY SUBJID; /*因为本实验中SUBJID已经是唯一的了，所以可以作为合并依赖变量。*/
	IF b; 
	/*为什么要merge DM？因为要用到DM里的reference即RFSTDTC。
	  注意，因DM里一定是一个受试者一条record，而EX里可能会存在一个受试者多条records
	  的情况，所以“一对多”MERGE里DM在EX前边。而DM里受试者可能比EX多，所以用if b
	  选择存在于EX里的受试者进行处理。*/

    /*计算study day*/
	IF input(EXSTDAT,yymmdd10.)>= input(RFSTDTC,yymmdd10.) THEN 
	    EXSTDY = input(EXSTDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.)+1 ;
	ELSE EXSTDY = input(EXSTDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.);

	IF input(EXENDAT,yymmdd10.)>= input(RFSTDTC,yymmdd10.) THEN 
	    EXENDY = input(EXENDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.)+1 ;
	ELSE EXENDY = input(EXENDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.);

    /*创建--DTC变量（ISO 8601）*/
	EXSTDTC = put(input(EXSTDAT,yymmdd10.),e8601da10.); /* 可以这样格式转换。但左侧变量必是新建的。*/
    EXENDTC = put(input(EXENDAT,yymmdd10.),e8601da10.);
	/*为什么--DTC变量的生成不放在最后？因为下一步调用宏%getSEQ的时候需要使用EXSTDTC作为自然基排序，而
	  ISO 8601虽然是char，但可以用于排序（因为它是标准值如“2016-08-07”）。而EXSTDAT也是char，
	  但不是标准的，如它的占位8-10位都有可能，这在排序时可能会排错。*/

    DROP RFSTDTC EXSTDAT EXENDAT ;
RUN;

/*添加EXSEQ变量。*/
%getSEQ(dsin=EX_date, dsout=EX_seq,domain=EX, keys=USUBJID EXTRT EXSTDTC);

/*此DATA步用于将前边的所有数据集（包括空表）merge到一起，
  得到最终的SDTM EX表。*/
DATA GINSDTM.EX ;
    MERGE GINMAP.EX_blank EX_basic EX_date EX_seq ;
	BY SUBJID;
	DROP EXSTDAT EXENDAT;
RUN;

```  
&ensp;&ensp;&ensp;&ensp;   
## 5  EF自定义域  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;GetEF.sas  
```  
/***********************************************************
 下方代码用于生成SDTM里的EF数据集。
 EF域是我自定义的类似finding domains的域，用于存储每次visit的
 MBI,MPI,MGI测试结果。
 ***********************************************************/


/*调用宏%getblank生成 空白数据集 GINMAP.EF_blank 。*/
OPTIONS MSTORED SASMSTORE=GINMacro ;
%getblank(maptable=GINMAP.EF_map , dsout=GINMAP.EF_blank);

/*此DATA步将原始数据集EF_raw的横向格式转变为纵向格式，返回EF_V数据集。*/
/*先处理visit 1的数据*/
PROC TRANSPOSE data=GINRaw.EF_raw(keep=SUBJID EFDAT_Base MBI_Base MGI_Base MPI_Base 
                                    rename=(EFDAT_Base=EFDAT MBI_Base=MBI MGI_Base=MGI 
                                            MPI_Base=MPI))
				 out=EF_visit1(drop=_label_ rename=(_name_=EFTESTCD COL1=EFORRES_num));
BY SUBJID EFDAT;
VAR MBI MGI MPI;
RUN;

/*增加变量VISIT，值为"VISIT 1"*/
DATA EF_visit1;
    SET EF_visit1;
	VISIT = "VISIT 1";
RUN;

/*同样方式处理visit 2，visit 3的数据*/
PROC TRANSPOSE data=GINRaw.EF_raw(keep=SUBJID EFDAT_2W MBI_2W MGI_2W MPI_2W 
                                    rename=(EFDAT_2W=EFDAT MBI_2W=MBI MGI_2W=MGI MPI_2W=MPI))
				 out=EF_visit2(drop=_label_ rename=(_name_=EFTESTCD COL1=EFORRES_num));
BY SUBJID EFDAT;
VAR MBI MGI MPI;
RUN;

DATA EF_visit2;
    SET EF_visit2;
	VISIT = "VISIT 2";
RUN;

PROC TRANSPOSE data=GINRaw.EF_raw(keep=SUBJID EFDAT_4W MBI_4W MGI_4W MPI_4W 
                                    rename=(EFDAT_4W=EFDAT MBI_4W=MBI MGI_4W=MGI MPI_4W=MPI))
				 out=EF_visit3(drop=_label_ rename=(_name_=EFTESTCD COL1=EFORRES_num));
BY SUBJID EFDAT;
VAR MBI MGI MPI;
RUN;

DATA EF_visit3;
    SET EF_visit3;
	VISIT = "VISIT 3";
RUN;

/*将EF_visit1，EF_visit2，EF_visit3上下合并，并按自然基排序，
  得到EF_V*/
DATA EF_V ;
    SET EF_visit1 EF_visit2 EF_visit3;
    BY SUBJID VISIT EFTESTCD;
RUN;

/*此DATA步修改原始纵向数据集EF_V里的基本变量，返回EF_basic数据集。*/
DATA EF_basic;
    SET EF_V ;
	STUDYID = "1001";
	DOMAIN = "EF";
	SITEID = "01";
	USUBJID = trim(left(STUDYID))||"-"||trim(left(SITEID))||"-"||trim(left(SUBJID)) ;
	
	/*创建EFTEST*/
	IF EFTESTCD="MBI" THEN EFTEST="Mean Bleeding Index";
	ELSE IF EFTESTCD="MGI" THEN EFTEST="Mean Gingival Index";
	ELSE IF EFTESTCD="MPI" THEN EFTEST="Mean Plaque Index";

	/*创建结果变量*/
	/*注意，不能直接用EFORRES=put(EFORRES_num,4.2);因为若某行的EFORRES_num为空，
	  则此公式得到的EFORRES不是空，而是两位的字符串" ."，即一个空格加一个点号。但
	  这不是我们想要的结果，我们希望EFORRES_num为空的时候，EFORRES也为空。否则后边
	  要对EFORRES变量进行判断的时候会出问题。(4.2是数字的格式，占4位，其中含2位小数)*/
	IF EFORRES_num^=. THEN EFORRES = put(EFORRES_num,4.2); 
	ELSE EFORRES="";
	EFSTRESC = EFORRES;/*此三指标都没有单位，所以也就无标准单位*/
	IF EFORRES^="" THEN EFSTRESN=input(EFORRES,best.);
	ELSE EFSTRESN=. ;

	/*创建VISITNUM变量*/
	IF VISIT="VISIT 1" THEN VISITNUM=1 ;
	ELSE IF VISIT="VISIT 2" THEN VISITNUM=2 ;
	ELSE IF VISIT="VISIT 3" THEN VISITNUM=3 ;

	/*创建EFBLFL指出基线records*/
	IF VISIT="VISIT 1" THEN EFBLFL="Y" ; ELSE EFBLFL="";

	/*创建EPOCH变量*/
	IF VISIT="VISIT 1" THEN EPOCH = "SCREENING";
	ELSE IF VISIT="VISIT 2" OR VISIT="VISIT 3" THEN EPOCH = "TREATMENT";

	DROP EFORRES_num SITEID ;
RUN;


/*此步DATA创建EFSTAT，EFREASND变量，返回EF_stat*/
DATA EF_stat;
    SET EF_basic ;
	LENGTH EFSTAT $20;
	LENGTH EFREASND $50;
	IF missing(EFORRES) THEN EFSTAT="NOT DONE";
	ELSE EFSTAT="";

	/*受试者的test没做有两种原因：在visit1发生的是因为screen failure，
	  在visit2/3发生的是因为subject moved.*/
	IF EFSTAT="" THEN EFREASND="";
	ELSE IF VISIT in ("VISIT 2","VISIT 3") AND EFSTAT="NOT DONE" THEN 
    EFREASND="SUBJECT MOVED";
	ELSE IF VISIT="VISIT 1" AND EFSTAT="NOT DONE" THEN EFREASND="SCREEN FAILURE";

    retain n;
	IF VISIT="VISIT 1" AND EFSTAT="NOT DONE" THEN n=USUBJID;
	IF USUBJID=n THEN EFREASND="SCREEN FAILURE";
	/*后边这一步是必须的，若没有，则会出现，screen failure的受试者在visit2/3时的
	  EFREASND是subject moved，但这是不对的，应该还是screen failure*/
    DROP n;
RUN;

/*以下DATA步获得日期变量*/
DATA EF_date ;

	MERGE GINSDTM.DM(keep=SUBJID RFSTDTC in=a)  
          EF_stat(keep=SUBJID USUBJID VISITNUM EFTESTCD EFDAT in=b);
	BY SUBJID; /*因为本实验中SUBJID已经是唯一的了，所以可以作为合并依赖变量。*/
	IF b; /*MERGE里DM放前边是为保证“一对多”，此处if b选择在EF里的受试者*/

	/*计算study day*/
	IF input(EFDAT,yymmdd10.)>= input(RFSTDTC,yymmdd10.) THEN 
	    VISITDY = input(EFDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.)+1 ;
	ELSE VISITDY = input(EFDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.);
	/*其实严格来讲这样不对，因为原数据集里EFDAT是受试者实际参加检查的日期，即
	  实际visit日期。而VISITDY是方案预设的visit日期，两者不一样。但本次实验
	  实际日期等于预设日期，所以可以这样做。*/
    
	/*转换ISO 8601格式*/
	EFDTC = put(input(EFDAT,yymmdd10.),e8601da10.); /* 可以这样格式转换。但左侧变量必是新建的。*/

	DROP EFDAT ;
RUN;

/*添加EXSEQ变量。*/
%getSEQ(dsin=EF_date, dsout=EF_seq,domain=EF, keys=USUBJID VISITNUM EFTESTCD);

/*将空表与上方得到的表格合并*/
DATA EF_ALL ;
    MERGE GINMAP.EF_blank EF_basic EF_stat EF_date EF_seq ;
	BY SUBJID;
	DROP EFDAT RFSTDTC;
	/*此MERGE属于多对多的merge，但因为每个表的SUBJID都是排过序的且每位受试者都占9行，
	  所以事实上实现的是一对一合并*/
RUN;

/*以下是调用宏%DelScreenFailure将EF_ALL里的screen failure的records删掉
  生成数据集GINSDTM.EF。
  因为EF域主要为了功效分析，而screen failure的受试者基线数据不参与分析。*/
%DelScreenFailure;


```  
&ensp;&ensp;&ensp;&ensp;   
## 6  DS  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;GetDS.sas  
```  
/***********************************************************
 下方代码用于生成SDTM里的DS数据集。
 ***********************************************************/


/*调用宏%getblank生成 空白数据集 GINMAP.DS_blank 。*/
OPTIONS MSTORED SASMSTORE=GINMacro ;
%getblank(maptable=GINMAP.DS_map , dsout=GINMAP.DS_blank);

/*此DATA步将原始数据集DS_raw的横向格式转变为纵向格式，返回DS_V数据集。*/
PROC TRANSPOSE data=GINRaw.DS_raw
				 out=DS_V(drop=_label_ rename=(_name_=Disposition COL1=DSTERM));
BY SUBJID;
VAR INFORMED_CONSENT RANDOMIZED  VISIT_2 VISIT_3;
RUN;

/*此DATA步修改原始纵向数据集DS_V里的基本变量，返回DS_basic数据集。*/
DATA DS_basic;
    SET DS_V ;
	STUDYID = "1001";
	DOMAIN = "DS";
	SITEID = "01";
	USUBJID = trim(left(STUDYID))||"-"||trim(left(SITEID))||"-"||trim(left(SUBJID)) ;

	DROP SITEID Disposition DSTERM;
RUN;

PROC SORT data = DS_basic out = DS_basic NODUPKEY;
    BY SUBJID;
RUN;


/*下述DATA步用于修改DSTERM并创建DSDECOD，DSCAT,DSSCAT, EPOCH, DSSEQ变量*/
DATA DS_term ;
    SET DS_V;
	/*所有受试者都做了知情同意*/
	IF Disposition="INFORMED_CONSENT" AND DSTERM="INFORMED CONSENT OBTAINED" THEN DO;
	    DSDECOD = "INFORMED CONSENT OBTAINED"; 
        DSCAT="PROTOCOL MILESTONE"; 
		EPOCH = "SCREENING";
		DSSEQ=1;
        END;
	/*正常完成所有阶段的受试者的情况*/
	ELSE IF Disposition="RANDOMIZED" AND DSTERM="RANDOMIZED" THEN DO;
	    DSDECOD ="RANDOMIZED";
        DSCAT="PROTOCOL MILESTONE"; 
        EPOCH = "SCREENING";
		DSSEQ=2;
        END;
	ELSE IF Disposition IN ("VISIT_2","VISIT_3") AND DSTERM="COMPLETED" THEN DO;
	    DSDECOD ="COMPLETED";
        DSCAT="DISPOSITION EVENT";
        DSSCAT="STUDY PARTICIPATION";
		EPOCH = "TREATMENT";
		DSSEQ=4;
		END;
	/*subject moved的情况*/
	ELSE IF Disposition IN ("VISIT_2","VISIT_3") AND DSTERM="SUBJECT MOVED" THEN DO;
	    DSDECOD ="LOST TO FOLLOW-UP";
        DSCAT="DISPOSITION EVENT";
        DSSCAT="STUDY PARTICIPATION";
		EPOCH = "TREATMENT";
		DSSEQ=4;
		END;
	/*screen failure的情况。flag用于指示以后删掉该行*/
	ELSE IF Disposition IN ("VISIT_2","VISIT_3") AND DSTERM="" THEN flag="Y";
	/*为所有受试者填写screening这个epoch的完成情况，即插入一条record*/
	/*以下为screening failure的受试者填写，他们不用插入新行，因为改写旧行已能实现目的。
	  而screening顺利完成的受试者，需要插入新行，由宏%AddScreenDisposition完成。*/
	ELSE IF Disposition="RANDOMIZED" AND DSTERM="SCREEN FAILURE" THEN DO;
	    DSTERM = "INCLUSION CRITERIA NOT MET";
	    DSDECOD ="SCREEN FAILURE";
        DSCAT="DISPOSITION EVENT"; 
		DSSCAT="STUDY PARTICIPATION";
        EPOCH = "SCREENING";
		DSSEQ=2;
        END;
RUN;

/*调用此宏，返回得到的DS_term里，所有完成了screen epoch的受试者，
  都被添加了一行record记录screen epoch完成了。*/
%AddScreenDisposition;

/*然后需要解决每个受试者的records的排序问题，通过DSSEQ。
  每个受试者的records都已经通过赋值DSSEQ给出了序号。
  但是，有一些问题。每个完成所有流程的受试者，其visit2和
  visit3各有一条record，除Disposition变量不同外，其它内容
  一样。而subject moved的受试者，不论其从visit2还是visit3
  开始失访，其treatment epoch都是没有完成的，各有一条record。
  而screen failure的受试者，其visit2和visit3虽然没有意义，
  但目前仍各有一条record。*/

	PROC SORT data=DS_term out=DS_term;
	    BY SUBJID DSSEQ Disposition; 
		/*Disopsition主要负责每个受试者vist2和visit3的排序*/
	RUN;

/*添加日期变量。
  为什么要保留每个受试者的visit2和visit3两行？就是为了方便
  添加日期变量。*/
/*以下DATA步主要收集了每个受试者的disposition完成是在哪次
  visit，用disposvisit表示。*/
DATA DS_disposvisit(keep=SUBJID disposvisit);
    SET GINRaw.DS_raw;
	IF INFORMED_CONSENT="INFORMED CONSENT OBTAINED" AND 
       RANDOMIZED="SCREEN FAILURE" 
    THEN disposvisit="V1";
	ELSE IF INFORMED_CONSENT="INFORMED CONSENT OBTAINED" AND 
            RANDOMIZED="RANDOMIZED" AND
			VISIT_2="SUBJECT MOVED" AND VISIT_3="SUBJECT MOVED" 
	THEN disposvisit="V2";/*V2时才能发现此受试者失访*/
	ELSE IF INFORMED_CONSENT="INFORMED CONSENT OBTAINED" AND 
            RANDOMIZED="RANDOMIZED" AND
			VISIT_2="COMPLETED" AND VISIT_3="SUBJECT MOVED" 
	THEN disposvisit="V3";/*V3时才能发现此受试者失访*/
	ELSE IF INFORMED_CONSENT="INFORMED CONSENT OBTAINED" AND 
            RANDOMIZED="RANDOMIZED" AND
			VISIT_2="COMPLETED" AND VISIT_3="COMPLETED" 
	THEN disposvisit="V3";/*V3时才能发现此受试者完成treatment*/
RUN;

DATA DS_date;
	    MERGE GINRaw.EF_raw(keep=SUBJID EFDAT_Base EFDAT_2W EFDAT_4W in=a)  
              DS_term(in=b)
              DS_disposvisit
              GINSDTM.DM(keep=SUBJID RFSTDTC); 
		BY SUBJID;
		IF b;
		/*此merge选择了EF原始数据，因为它记录的每个受试者的每次实际visit日期，
		  这个时间是计算DS里日期需要的，因为DS里记录的也是实际日期。另外，
		  EF原始数据正好是一个受试者一行，放在前边即为“一对多”。然后用
		  IF b；选择DS里的受试者，因为现在制备的是DS数据集。另外，DS_disposvisit
		  是为了分辨受试者treatment epoch的结束日期。DM是为了引入RFSTDTC*/
        
		/*添加时间变量*/
		/*知情同意日期为visit1实际日期，即EFDAT_Base*/
		IF DSTERM="INFORMED CONSENT OBTAINED" THEN DSSTDAT = EFDAT_Base;
		/*随机化不论完成与否，日期都为visit1实际日期，即EFDAT_Base*/
		IF DSTERM="RANDOMIZED" THEN DSSTDAT = EFDAT_Base;
		/*screen epoch不论完成与否，结束日期都为visit1实际日期，即EFDAT_Base*/
		IF EPOCH="SCREENING" AND DSCAT="DISPOSITION EVENT" THEN
		DSSTDAT = EFDAT_Base;
		/*visit2和visit3都是treatment epoch，而此epoch完成与否只需要一条
		  record记录即可。可参考DS_disposvisit。在此两行里，disposvisit变量
		  值只可能是V2或V3，若是V2，则代表treatment epoch结束日期应为visit2
		  的实际日期。若为V3，则代表treatment epoch结束日期应为visit3
		  的实际日期。*/
		IF EPOCH="TREATMENT" AND disposvisit="V2" THEN DSSTDAT = EFDAT_2W;
		ELSE IF EPOCH="TREATMENT" AND disposvisit="V3" THEN DSSTDAT = EFDAT_4W;

		/*创建study day变量DSSTDY和ISO 8601变量DSSTDTC*/
		DSSTDTC = put(input(DSSTDAT,yymmdd10.),e8601da10.);

		IF input(DSSTDAT,yymmdd10.)>= input(RFSTDTC,yymmdd10.) THEN 
	    DSSTDY = input(DSSTDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.)+1 ;
	    ELSE DSSTDY = input(DSSTDAT,yymmdd10.)- input(RFSTDTC,yymmdd10.);

RUN;

	/*因为之前提到，指示每个受试者的treatment epoch完成与否的record只需要
	  一条即可，但DS_term里有两条，但好在其它变量内容一样，所以删掉一条即可。
	  此处删掉visit2对应的那条。另外，flag="Y"是对screen failure受试者
	  的多余records的标记，也要删掉。*/
DATA DS_date;
	    SET DS_date;
		IF Disposition="VISIT_2" OR flag="Y" THEN DELETE;
		KEEP SUBJID DSTERM DSDECOD DSCAT DSSCAT EPOCH DSSEQ DSSTDTC DSSTDY;
RUN;

	/*最后，需要把之前的数据集（包括空表）merge起来，得到最终数据集*/
DATA GINSDTM.DS ;
    MERGE GINMAP.DS_blank DS_date(in=a) DS_basic  ;
	BY SUBJID ;
	IF a;/*因为DS_date里已经对行进行好了筛选，尤其是screen failure受试者，
	       应该只有两行，但是其它数据集里这些受试者每位可能有4行。*/
	DROP Disposition;
RUN;

```  
&ensp;&ensp;&ensp;&ensp;    

# 四  SDTM结果数据集展示  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;  以上代码共得到DM, SUPPDM, EX, EF, DS 五个SAS数据集。结果展示如下：  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;DM   
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;SUPPDM  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;EX  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;EF  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;     
&ensp;&ensp;&ensp;&ensp;DS  
&ensp;&ensp;&ensp;&ensp; 
  
&ensp;&ensp;&ensp;&ensp;   

# 五  宏代码  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;本次试验使用的宏代码展示如下：  
&ensp;&ensp;&ensp;&ensp;  
## 1  宏%getblank  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;getblank.sas  
```  
/********************************************************************************
 此宏的目的是参考某域的map表，生成此域在最终SDTM表里需要包含的变量（名称、标签、类型、
 长度），且顺序是此map表里变量的顺序，用于和之后得到的此域的各种分表merge到一起，
 得到此域的SDTM表，并让表里的变量合乎map里指定的规则。
*********************************************************************************/


OPTIONS MSTORED SASMSTORE=GINMACRO;
%MACRO getblank(maptable= , dsout= ) / STORE SOURCE;  
    /*maptable是某域的map表，即SAS数据集。dsout是你想得到的空表数据集名字，可带库名。*/

    /*_n_是 set的循环次数，是数值。通过set循环，为map表里Variable_Name列里的每个变量
      都生成相应的宏变量 Variable_Name_n，其标签、类型、长度也分别是宏变量Variable_Label_n, 
      Type_n, Length_n
    */
    DATA  _null_;
	    SET &maptable. nobs=n ; /*nobs用于获取maptable的条目数，即其对多少变量制定规则*/
		CALL SYMPUTX('TOTAL',n);
		CALL SYMPUTX("Variable_Name_"||left(put(_n_, best.)),Variable_Name);
		CALL SYMPUTX("Variable_Label_"||left(put(_n_, best.)),Variable_Label);
		CALL SYMPUTX("Length_"||left(put(_n_,best.)),Length);
	RUN;

    DATA &dsout. ;  
	    %DO i=1 %TO &TOTAL. ;
		    ATTRIB &&Variable_Name_&i  LABEL="&&Variable_Label_&i" LENGTH=&&Length_&i. ;
            /*attrib语句，设定一个或更多变量的输入输出格式，标签，长度。*/
			/*注意这个LENGTH。map表里，字符型变量的length用$w 表示，而数值型型变量的length用w 表示，
		      读到宏变量Length_i 里，都成了字符串。但最后生成的空表里各变量的类型是对的，
		      为什么？因为此处的attrib语句，设置LENGTH的时候，是解析的宏语句，所以解析得到
			  LENGTH=$20.这样的，得到字符型变量，解析得到LENGTH=8.这样的，得到数值型变量。
			  所以上边无需提取map表里的Type变量，只需提取Length即可*/
        %END;
		DELETE; /*如果没有delete; 则得到的表会有一行空行。*/
	RUN;
	

%MEND getblank;

```   
&ensp;&ensp;&ensp;&ensp;  
## 2  宏%getSEQ  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;getSEQ.sas  
```  
/********************************************************************************
 此宏的目的是为某个数据集生成--SEQ变量。
 宏参数：dsin是输入数据集，dsout是包含了比原来多一列--SEQ的数据集（带库名）。domain
 是域的2字符代号。keys是此域的自然基里你需要用于排序的部分，用空格隔开即可。
 注意，--SEQ变量一般是为了在一个数据集里，让同一个受试者的records能够拥有唯一的代号。
 但同一个受试者的records的顺序应如何？因--SEQ常作为自然基的替代，所以--SEQ需保证其对
 同一受试者records的排序和自然基排出的顺序一致。
*********************************************************************************/


OPTIONS MSTORED SASMSTORE=GINMACRO;
%MACRO getSEQ(dsin=, dsout=, domain=, keys=) / STORE SOURCE;

    /*先按照你提供的自然基对dsin数据集进行排序*/
    PROC SORT data=&dsin out=seq_temp ;
	    BY &keys;
	RUN;

	/*为排序后的数据集添加--SEQ变量并赋值，然后保存在seq_temp数据集里。
	  一定注意，getSEQ函数返回的是一个数据集*/
	DATA &dsout ;
	    SET seq_temp;
		BY &keys; /*此BY用于提供first.XX变量*/
		domain = "%upcase(&domain)";
		IF first.USUBJID THEN DO;
		    &domain.SEQ = 0 ;
		END;
		&domain.SEQ + 1;
    RUN;
%MEND getSEQ;

```   
&ensp;&ensp;&ensp;&ensp;  
## 3  宏%DelScreenFailure  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;DelScreenFailure.sas  
```  
/********************************************************************************
 此宏的目的是为专门为EF_ALL数据集删去screen failure的受试者的records的。
 其中GINSDTM.EF即生成的SDTM EF数据集，内不含screen failure受试者
*********************************************************************************/


OPTIONS MSTORED SASMSTORE=GINMACRO;
%MACRO DelScreenFailure / STORE SOURCE;
    PROC SQL noprint;
	    SELECT DISTINCT SUBJID into:SCRFID separated by ","
		FROM work.EF_ALL
		WHERE EFREASND="SCREEN FAILURE"
        ;
		/*此SQL使用宏变量SCRFID收集screen failure的受试者的SUBJID*/
	QUIT;
	%PUT &SCRFID; /*日志中显示： 31,32 */

    DATA GINSDTM.EF;
	    SET work.EF_ALL;
		IF SUBJID IN (&SCRFID) THEN DELETE;
	RUN;

%MEND DelScreenFailure ;

```   
&ensp;&ensp;&ensp;&ensp;  
## 4  宏%AddScreenDisposition  
&ensp;&ensp;&ensp;&ensp;  
* **代码**：   
&ensp;&ensp;&ensp;&ensp;AddScreenDisposition.sas  
```    
/********************************************************************************
 此宏的目的是为专门为DS_term数据集里完成screening epoch的受试者增添一行记录“完成
 screening epoch”的record的。
*********************************************************************************/


OPTIONS MSTORED SASMSTORE=GINMACRO;
%MACRO AddScreenDisposition / STORE SOURCE;
    %LOCAL i;

    PROC SQL noprint;
	    SELECT DISTINCT SUBJID into:CSCRID separated by " "
		FROM work.DS_term
		WHERE Disposition="RANDOMIZED" AND DSTERM="RANDOMIZED"
        ;
		/*此SELECT过程用于选出完成了screen epoch的所有受试者，将其SUBJID
		  存储于CSCRID宏变量，之间由逗号隔开。*/
		SELECT COUNT(DISTINCT SUBJID) into:CSCRID_count
		FROM work.DS_term
		WHERE Disposition="RANDOMIZED" AND DSTERM="RANDOMIZED"
        ;
		/*此SELECT过程得到完成screen epoch的受试者数目，存于
		  CSCRID_count宏变量*/
	QUIT;
	%PUT &CSCRID; /*从1号到30号都完成了screen epoch，只有31和32号因
	                screen failure未进行randomization，从而位完成
	                screen epoch。*/
	%PUT &CSCRID_count; /*从1号到30号共30个*/

	%DO i=1 %TO &CSCRID_count ;
	    %LET SID =%scan(&CSCRID,&i) ;
		/*注意，这里有一个问题，如果宏变量CSCRID不是空格分隔而是逗号分隔，
		  则直接这样调用会出现“宏函数 %SCAN 的参数过多”的ERROR提示，这是
		  因为解析宏变量CSCRID后，程序将逗号理解为给%scan函数提供的参数的
		  逗号，当然参数太多，此时可以使用%bquote函数。参：
		  http://www.epiman.cn/thread-36646-1-1.html */
		%PUT ****&SID**** ;
		/*下为为表DS_term里相应的受试者插入record*/
		PROC SQL;
		    INSERT INTO DS_term 
			SET SUBJID="&SID",
			    DSTERM="COMPLETED",
				DSDECOD="COMPLETED",
				DSCAT="DISPOSITION EVENT",
				DSSCAT="STUDY PARTICIPATION",
				EPOCH="SCREENING",
				DSSEQ=3;
				
		QUIT;	    
	%END;

%MEND AddScreenDisposition;

```   
