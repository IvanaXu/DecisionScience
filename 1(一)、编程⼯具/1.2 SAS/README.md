
- [1.2.1 SAS之禅](#121-sas之禅)
- [1.2.2 SAS教程](#122-sas教程)
  - [1.2.2.1 SAS基础](#1221-sas基础)
  - [1.2.2.2 SAS拼表](#1222-sas拼表)
  - [1.2.2.3 SQL应用](#1223-sql应用)
  - [1.2.2.4 PROC步](#1224-proc步)
  - [1.2.2.5 SAS进阶](#1225-sas进阶)
  - [1.2.2.6 SAS宏](#1226-sas宏)
- [1.2.3 SAS书籍](#123-sas书籍)
- [1.2.4 SAS Help使⽤](#124-sas-help使)
  - [1.2.4.1 通篇阅读](#1241-通篇阅读)
  - [1.2.4.2 关键字搜索](#1242-关键字搜索)
  - [1.2.4.3 归纳体系](#1243-归纳体系)
  - [1.2.4.4 总结](#1244-总结)
- [1.2.5 SAS IDE](#125-sas-ide)
  - [1.2.5.1 SAS-BASE](#1251-sas-base)
  - [1.2.5.2 SAS-EG](#1252-sas-eg)
  - [1.2.5.3 SAS-EM](#1253-sas-em)
  - [1.2.5.4 SAS-University](#1254-sas-university)
- [1.2.6 SAS代码规范](#126-sas代码规范)
- [1.2.7 SAS⽚段](#127-sas段)
  - [](#)
  - [～格式刷](#格式刷)
  - [～程序休眠](#程序休眠)
  - [～条件构建宏](#条件构建宏)
  - [～FTP下载/上传文件](#ftp下载上传文件)
  - [～月度循环](#月度循环)
  - [～DO OVER](#do-over)
  - [～格式化加强理解](#格式化加强理解)
  - [～PROC SUMMARY](#proc-summary)
  - [～哈希表HASH连接](#哈希表hash连接)
  - [～排列组合](#排列组合)
  - [～Lift提升度](#lift提升度)
  - [～PROC SQL](#proc-sql)
  - [～SAS绘图](#sas绘图)
  - [～自制数据集](#自制数据集)
  - [～宏变量工厂](#宏变量工厂)
  - [～判断表是否存在](#判断表是否存在)
  - [～KS计算](#ks计算)
  - [～批量SQL(宏)](#批量sql宏)
- [1.2.8 SAS编译](#128-sas编译)

### 1.2.1 SAS之禅
> 简于理解，精于效率。


### 1.2.2 SAS教程

- 第1课 SAS基础入门
- 第2课 SAS拼表
- 第3课 SQL应用
- 第4课 PROC步
- 第5课 SAS进阶
- 第6课 SAS数据清洗
- 第7课 SAS宏
教程为2019.03课程原稿，相对入门，可用以温故知新。

以下摘要之：
#### 1.2.2.1 SAS基础

```SAS
OPTIONS COMPRESS = YES;
/*	libname ana "";*/

DATA A;
SET SASHELP.CARS;
RUN;

DATA A;
SET SASHELP.CARS;
MSRP1 = MSRP + 1;
RUN;

DATA A;
KEEP MSRP1;
SET SASHELP.CARS;
MSRP1 = MSRP + 1;
RUN;

DATA B;
FORMAT MSRP2 $20.;
SET A;
/* IF MSRP1 > 30000 THEN MSRP2 = "DAYU3W"; */
/* ELSE MSRP2 = "XIAOYU1000"; */
IF MSRP1 > 30000 THEN MSRP2 = "DAYU3W";
ELSE IF MSRP1 > 1000 THEN MSRP2 = "DAYU3W<1000";
ELSE IF MSRP1 > 200 THEN MSRP2 = "DAYU3W<200";
ELSE MSRP2 = "XIAOYU1000";
RUN;

DATA A;
KEEP MSRP1;
SET SASHELP.CARS;
MSRP1 = MSRP + 1;
IF MSRP1 > 50000; 
RUN;

DATA A;
KEEP MSRP1;
SET SASHELP.CARS;
MSRP1 = MSRP + 1;
WHERE MSRP1 > 50000; 
RUN;

DATA C;
KEEP MSRP1 MSRP2;
SET SASHELP.CARS;
MSRP1 = MSRP + 1;
IF MSRP1 > 50000; 
IF MSRP1 > 60000 THEN MSRP2 = ">";
ELSE MSRP2 = "<";
RUN;

/* IF SUBSTR(A,1,1) = "1" */
/* IF SUBSTR(A,1,1) = "1" OR SUBSTR(A,1,1) = "2" */
/* IF SUBSTR(A,1,1) = "1" AND MSRP1 > 60000 */
/* IF SUBSTR(A,1,1) IN ("1", "2") */
/* IF SUBSTR(A,1,1) NOT IN ("1", "2") */
/* "1" */
/* "2" */
/* "3" */

DATA D;
SET C C;
RUN;

PROC SORT DATA = D;
BY MSRP2 MSRP1;
RUN;

PROC FREQ DATA = D;
TABLE MSRP2 * MSRP1;
RUN;
```

#### 1.2.2.2 SAS拼表

```SAS
OPTIONS COMPRESS = YES;

DATA TEST_X1;
INPUT NAME $ PRODUCT $ TYPE $;
CARDS;
A CAR 40
B CAR 42
C BUS 44
D MOTO 9
E BUS 10
;
RUN;

DATA TEST_X2;
INPUT NAME $ PRODUCT $ TYPE $;
CARDS;
A CAR 40
C CAR 42
B BUS 44
H MOTO 9
J BUS 10
;
RUN;

DATA TEST_Y;
INPUT NAME $ NPRODUCT $ NTYPE $;
CARDS;
A APPLE 38
B BANANA 42
C CAT 44
D DOG 9
E EGG 10
;
RUN;

DATA TEST_Z;
INPUT NAME $ NPRODUCT $ NTYPE $;
CARDS;
A APPLE 38
A BANANA 42
B CAT 44
B DOG 9
B EGG 10
;
RUN;

DATA A1_1;
SET TEST_X1 TEST_X2;
RUN;

DATA A1_2;
SET TEST_X1 TEST_Y;
RUN;

DATA A1_3;
SET TEST_X1 TEST_Y(RENAME=(NPRODUCT=PRODUCT NTYPE=TYPE));
RUN;

/* DATA t3; */
/* MERGE t1(IN=A) t2(IN=B); */
/* BY n1; */
/* IF A; */
/* IF B; */
/* RUN; */

PROC SORT DATA = TEST_X2;BY NAME;RUN;
PROC SORT DATA = TEST_Y;BY NAME;RUN;
PROC SORT DATA = TEST_Z;BY NAME;RUN;

DATA A2;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF A;
RUN;

DATA A2_1;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF NOT A;
RUN;

DATA A3;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF B;
RUN;

DATA A4;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF A AND B;
RUN;

DATA A5;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
RUN;

DATA A51;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF A OR B;
RUN;

DATA A6;
MERGE TEST_X2(IN=A) TEST_Y(IN=B);
BY NAME;
IF A;
RUN;

DATA A7;
MERGE TEST_X2(IN=A) TEST_Z(IN=B);
BY NAME;
IF A;
RUN;
```

#### 1.2.2.3 SQL应用

```SAS
OPTIONS COMPRESS = YES;

/* CREATE */
PROC SQL;
CREATE TABLE TCUSTR(
    CUSTR_NBR VARCHAR(18),
    SEX INT
);
QUIT;

/* INSERT */
PROC SQL;
INSERT INTO TCUSTR(CUSTR_NBR, SEX)
    VALUES("440101200109090011", 1)
    VALUES("360101199901010012", 0);
QUIT;

/* DELETE */
PROC SQL;
DELETE FROM TCUSTR WHERE CUSTR_NBR = "440101200109090011";
QUIT;

/* UPDATE */
PROC SQL;
UPDATE TCUSTR SET SEX = 1 WHERE CUSTR_NBR = "360101199901010012";
QUIT;

/* SELECT */
PROC SQL;
SELECT * 
FROM SASHELP.CARS;
QUIT;

PROC SQL;
SELECT * 
FROM SASHELP.CARS 
WHERE MAKE = "Acura";
QUIT;

PROC SQL;
SELECT MAKE, MSRP
FROM SASHELP.CARS 
WHERE MAKE = "Acura";
QUIT;

/* SQL FUNC */
PROC SQL;
SELECT 
    COUNT(MSRP),
    SUM(MSRP),
    MAX(MSRP),
    MIN(MSRP),
    AVG(MSRP)
FROM SASHELP.CARS 
WHERE MAKE = "Acura";
QUIT;

PROC SQL;
SELECT 
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP
FROM SASHELP.CARS 
WHERE MAKE = "Acura";
QUIT;

/* GROUP BY */
PROC SQL;
SELECT 
    MAKE,
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP
FROM SASHELP.CARS
GROUP BY MAKE;
QUIT;

/* ORDER BY */
PROC SQL;
SELECT 
    MAKE,
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP 
FROM SASHELP.CARS
GROUP BY MAKE
ORDER BY CNT_MSRP;
QUIT;

/* WHERE */
PROC SQL;
SELECT 
    MAKE,
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP
FROM SASHELP.CARS
WHERE MAKE ^= "Acura"
GROUP BY MAKE
ORDER BY CNT_MSRP;
QUIT;

/* HAVING */
PROC SQL;
SELECT 
    MAKE,
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP
FROM SASHELP.CARS 
WHERE MAKE ^= "Acura"
GROUP BY MAKE
HAVING CNT_MSRP > 5
ORDER BY CNT_MSRP;
QUIT;

/* CREATE TABLE */
PROC SQL;
CREATE TABLE CARS_GROUP AS 
SELECT 
    MAKE,
    COUNT(MSRP) AS CNT_MSRP,
    SUM(MSRP) AS SUM_MSRP,
    MAX(MSRP) AS MAX_MSRP,
    MIN(MSRP) AS MIN_MSRP,
    AVG(MSRP) AS AVG_MSRP
FROM SASHELP.CARS 
WHERE MAKE ^= "Acura"
GROUP BY MAKE
HAVING CNT_MSRP > 5
ORDER BY CNT_MSRP;
QUIT;

/* CASE WHEN */
PROC SQL;
CREATE TABLE CARS_GROUP AS 
SELECT 
    MAKE,
    SUM(CASE WHEN ENGINESIZE > 3 THEN MSRP ELSE 0 END) AS SUM_MSRP
FROM SASHELP.CARS 
GROUP BY MAKE;
QUIT;
```

#### 1.2.2.4 PROC步

```SAS
OPTIONS COMPRESS = YES;

DATA CARS;
SET SASHELP.CARS;
RUN;


/* PROC SORT */
PROC SORT DATA = SASHELP.CARS;
BY MSRP;
RUN;

PROC SORT DATA = SASHELP.CARS OUT = CARS_MSRP;
BY DESCENDING MSRP;
RUN;
PROC SORT DATA = SASHELP.CARS OUT = CARS_MAKE;
BY MAKE;
RUN;

DATA CARS_DUP;
KEEP TYPE ORIGIN DRIVETRAIN;
SET SASHELP.CARS;
RUN;

PROC SORT DATA = CARS_DUP OUT = CARS_NDK NODUPKEY;
BY TYPE ORIGIN;
RUN;

PROC SORT DATA = CARS_DUP OUT = CARS_NQK NOUNIQUEKEY;
BY TYPE ORIGIN;
RUN;

PROC SORT DATA = CARS_DUP OUT = CARS_ND NODUP;
BY TYPE ORIGIN;
RUN;


/* PROC FREQ */
PROC FREQ DATA = CARS NOPRINT;
TABLE DRIVETRAIN/OUT=CARS_FREQ;
RUN;

PROC FREQ DATA = CARS;
TABLE DRIVETRAIN * ORIGIN/NOCOL NOROW NOPERCENT MISSING;
RUN;
PROC FREQ DATA = CARS;
TABLE ORIGIN * DRIVETRAIN/NOCOL NOROW NOPERCENT MISSING;
RUN;

PROC FREQ DATA = CARS;
TABLE DRIVETRAIN * ORIGIN;
WHERE TYPE = "SUV";
RUN;

PROC FREQ DATA = CARS;
TABLE TYPE * DRIVETRAIN * ORIGIN;
RUN;

PROC FREQ DATA = CARS;
TABLE (TYPE DRIVETRAIN) * ORIGIN;
/*	TABLE TYPE * ORIGIN;*/
/*	TABLE DRIVETRAIN * ORIGIN;*/
RUN;

/*	PROC SORT DATA = ;*/
/*	BY ;*/
/*	RUN;*/
/*	PROC FREQ DATA = ;*/
/*	TABLE ;*/
/*	RUN;*/
/*	PROC UNIVARIATE DATA = ;*/
/*	VAR ;*/
/*	RUN;*/


/* PROC UNIVARIATE */
PROC UNIVARIATE DATA = CARS;
VAR MSRP;
RUN;
PROC UNIVARIATE DATA = CARS;
VAR MAKE;
RUN;

PROC UNIVARIATE DATA = CARS;
VAR MSRP;
CLASS MAKE;
RUN;


/* PROC TRANSPOSE */
DATA CARS_MAMS;
KEEP MAKE TYPE MSRP;
SET SASHELP.CARS;
RUN;

PROC SORT DATA = CARS_MAMS OUT = CARS_MAMSD NODUPKEY;
BY MAKE TYPE;
RUN;

PROC TRANSPOSE DATA = CARS_MAMSD;
VAR MSRP;
BY MAKE;
RUN;

PROC TRANSPOSE DATA = CARS_MAMSD OUT=CARS_M PREFIX=ID_;
VAR MSRP;
BY MAKE;
ID TYPE;
RUN;


/* PROC SURVEYSELECT */
PROC SURVEYSELECT 
    DATA=CARS_MAMS METHOD=SRS N=3 
    OUT=CARS_SRS_N3;
RUN;
PROC SURVEYSELECT 
    DATA=CARS_MAMS METHOD=SRS SAMPRATE=0.1 
    OUT=CARS_SRS_P1;
RUN;

PROC SURVEYSELECT 
    DATA=CARS_MAMS METHOD=SRS SAMPRATE=0.1 
    OUT=CARS_SRS_N1;
STRATA MAKE;
RUN;
```

#### 1.2.2.5 SAS进阶

```SAS
OPTIONS COMPRESS = YES;

DATA CARS;
KEEP MAKE MSRP; 
SET SASHELP.CARS;
RUN;

PROC SORT DATA = CARS OUT = CARS_MSRP;
BY DESCENDING MSRP;
RUN;

PROC FREQ DATA = CARS_MSRP NOPRINT;
TABLE MAKE/OUT = CARS_FMAKE;
RUN;

/* SAS FORMAT */
DATA DEMOY;
INPUT NAME $11. BIRTH HEIGHT;
INFORMAT BIRTH YYMMDD10. HEIGHT 5.1;
CARDS;
LIXIAO      1959/10/21 170.5 
WANGMING    1992/02/21 177.8
;
RUN;
/*1960/01/01*/
/*1991/10/21 11616 */
/*1991/10/23 11618 */

DATA DEMON;
INPUT NAME $11. BIRTH $11. HEIGHT;
CARDS;
LIXIAO      1991/10/21 170.5
WANGMING    1992/02/21 177.8
;
RUN;

DATA DDATE;
SDATE = "01JAN2018"D;
F1DATE = SDATE;
F2DATE = SDATE;
F3DATE = SDATE;
RUN;

DATA DDATE;
SDATE = "01JAN2018"D;
FORMAT F1DATE YYMMDD10. F2DATE YYMMDD8. F3DATE YYMMDD6.;
F1DATE = SDATE;
F2DATE = SDATE;
F3DATE = SDATE;
RUN;

/* SAS DATE */
DATA SASDATE;
SDATE = "21DEC2018"D;
STIME = "09:39:00"T;
SDATETIME = "21DEC2018 09:39:00"DT;
RUN;

DATA SASDATE_TR;
SDATE = "21DEC2018"D;
STIME = "09:39:00"T;
SDATETIME = "21DEC2018 09:39:00"DT;

FORMAT F1DATE YYMMDD10. F2DATE YYMMDD8. FTIME TIME10. FDATETIME DATETIME20.;
F1DATE = SDATE;
F2DATE = SDATE;
FTIME = STIME;
FDATETIME = SDATETIME;
RUN;

/* INTNX */
DATA DINTNX;
SDATE = "01JAN2018"D;
FORMAT FDATE SDATED SDATEM SDATEY YYMMDD10.;
FDATE = SDATE;
SDATED = INTNX("DAY", SDATE, 1);
SDATEM = INTNX("MONTH", SDATE, 1); 
SDATEY = INTNX("YEAR", SDATE, 1);
RUN;

DATA DINTNXN;
SDATE = "02JAN2018"D;
FORMAT FDATE SDATEMB SDATEMM SDATEME SDATEMS YYMMDD10.;
FDATE = SDATE;
SDATEMB = INTNX("MONTH", SDATE, 1, "B");
SDATEMM = INTNX("MONTH", SDATE, 1, "M"); 
SDATEME = INTNX("MONTH", SDATE, 1, "E");
SDATEMS = INTNX("MONTH", SDATE, 1, "S");
RUN;

DATA DINTNXN;
SDATE = "02JAN2018"D;
FORMAT FDATE SDATEMB SDATEMM SDATEME SDATEMS YYMMDD10.;
FDATE = SDATE;
SDATEMB = INTNX("YEAR", SDATE, 1, "B");
SDATEMM = INTNX("YEAR", SDATE, 1, "M"); 
SDATEME = INTNX("YEAR", SDATE, 1, "E");
SDATEMS = INTNX("YEAR", SDATE, 1, "S");
RUN;

/* INTCK */
DATA DINTCK;
SDATE_M11 = "01DEC2018"D;
SDATE_M12 = "05NOV2018"D;
GAP_DAYS = INTCK("DAY", SDATE_M12, SDATE_M11);
GAP_MONS = INTCK("MONTH", SDATE_M12, SDATE_M11);
RUN;

/* PUT INPUT */
DATA DPUT;
DATE18 = 20181221;
DATEP18 = PUT(DATE18, $8.);
RUN;

DATA DPUT;
DATE18 = 20181221;
DATEP18 = PUT(DATE18, $8.);
DATEI18 = INPUT(DATEP18, YYMMDD8.);
/*	DATEI18 = INPUT(PUT(DATE18, $8.), YYMMDD8.);*/
DATEPN18 = PUT(DATEI18, YYMMDDN8.);
DATEIN18 = INPUT(DATEPN18, 8.);
RUN;
```

#### 1.2.2.6 SAS宏

```SAS
OPTIONS COMPRESS = YES;

DATA CARS;
KEEP MAKE MODEL MSRP;
SET SASHELP.CARS;
RUN;

DATA DVAR01;
SET CARS;
WHERE MAKE = "BMW";
RUN;
DATA DVAR02;
SET CARS;
WHERE MAKE = "BMW";
RUN;
DATA DVAR03;
SET CARS;
WHERE MAKE = "BMW";
RUN;

%LET VAR1 = BMW;
DATA DVAR01;
SET CARS;
WHERE MAKE = "&VAR1.";
RUN;
DATA DVAR02;
SET CARS;
WHERE MAKE = "&VAR1.";
RUN;
DATA DVAR03;
SET CARS;
WHERE MAKE = "&VAR1.";
RUN;
/* ... */
%PUT &VAR1.;

/* Acura */
/* BMW */
%LET VAR1 = BMW;
DATA DVAR01;
SET CARS;
WHERE MAKE = "&VAR1.";
RUN;

/* 1 */
%LET VAR1 = Acura;
/*	%PUT &VAR1.;*/
DATA DVAR1;
SET CARS;
WHERE MAKE = "&VAR1.";
RUN;

/* 2 */
DATA _NULL_;
CALL SYMPUT("VAR2", "Acura");
RUN;
/* %PUT &VAR2.; */
DATA DVAR2;
SET CARS;
WHERE MAKE = "&VAR2.";
RUN;

/* 3 */
/*	PROC SQL NOPRINT;*/
/*	SELECT MAKE INTO: VAR3 */
/*	FROM CARS;*/
/*	QUIT;*/
/*	PROC SQL NOPRINT;*/
/*	SELECT MAX(MAKE) INTO :VAR3 */
/*	FROM CARS;*/
/*	QUIT;*/
PROC SQL NOPRINT;
SELECT MAX(MAKE),MIN(MSRP) INTO :VAR3, :VAR4 
FROM CARS;
QUIT;
%PUT &VAR3. &VAR4.;

DATA DVAR3;
SET CARS;
WHERE MAKE = "&VAR3.";
RUN;

%MACRO T1;
    DATA A;
    RUN;
    %MEND T1;
%T1;

%MACRO T2(V=,);
    %PUT &V.;

    DATA DVAR_&V.;
    SET CARS;
    WHERE MAKE = "&V.";
    /* 100H */
    RUN;
%MEND T2;
%T2(V=BMW);
/*	%PUT &V.;*/
%T2(V=Acura);
/*	%PUT &V.;*/


%LET M = BMW;
%MACRO T3(V=,);
    %PUT &V.;
    %IF &V.= &M. %THEN %DO;
        DATA DVAR_&V.;
        SET CARS;
        WHERE MAKE = "&V.";
        /* 100H */
        RUN;
        /*			...*/
    %END;
    %ELSE %DO;
        DATA DVAR_&V.;
        SET CARS;
        WHERE MAKE = "&V." AND MSRP > 50000;
        /* 100H */
        RUN;
        /*			...*/
    %END;
%MEND T3;
%T3(V=BMW);
%T3(V=Acura);

/*	DATA A1;*/
/*	SET A;*/
/*	IF MSRP > 100 THEN A1 = 1;*/
/**/
/*	IF MSRP > 100 THEN DO;*/
/*		A1 = 1;*/
/*		A2 = 1;*/
/*	END;*/
/*	RUN;*/

DATA A;
DO I = 1 TO 10;
T = I + 2;
OUTPUT;
END;
RUN;

%MACRO T4(V=,);
    %PUT &V.;

    %DO I = 0 %TO &V.;
        DATA A_&I.;
        /*			SET ACCT;*/
        /*			WHERE MONTH = &I.; */
        RUN;
    %END;
%MEND T4;
%T4(V=10);
%T4(V=2);


%MACRO T5(V=,);
    %PUT &V.;

    DATA MSRP;
    SET CARS;
    %DO I = 0 %TO &V.;
    	IF MSRP > &I.*10000 THEN A&I. = 1;ELSE A&I. = 0;
    %END;
    RUN;
%MEND T5;
%T5(V=10);


/*EVAL*/
%LET S = 1;
%LET I = 10;
%PUT &S. &I.;

%LET S = &S. + &I.;
%PUT &S. &I.;

%LET S = 1;
%LET I = 10;
%LET S = %EVAL(&S. + &I.);
%PUT &S. &I.;
```

### 1.2.3 SAS书籍
> 《Learning SAS by Example: A Programmer's Guide》
>
> 《SAS编程技术教程（朱世武）》
>
> 《SAS编程与数据挖掘商业案例》
>
> 《SAS语言抛砖引玉》
>
> 《The Little SAS Book》
>
>  SAS Help and Documentation
>

### 1.2.4 SAS Help使⽤
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.0-000.png" width=800>
</p>

如上SAS书籍，专门强调了SAS Help and Documentation的重要性。
使用方法也有很多：

#### 1.2.4.1 通篇阅读
相信这个跟全文背诵一样恐怖，但跟着官方文档走，可更全面感受SAS产品设计。
如Contents就提供这样的阅读目录，可一览SAS Products.

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.1-000.png" width=400>
</p>

#### 1.2.4.2 关键字搜索
相信是最常用的方法，搜索框输入关键字，快速搜索。
如输入`substr`

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.2-000.png" width=400>
</p>

当然，也有缺点，尤其个别奇奇怪怪的关键字，如输入`index`

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.2-001.png" width=400>
</p>

搜索结果令人无所适从。

#### 1.2.4.3 归纳体系

关键字搜索非常有用，但当你忘记关键字拼写、拼错关键字甚至想要某某功能关键字时，归纳体系更加便捷。

（1）Category

以计算函数为例，搜索`Function Categories`

> Functions can be categorized by the types of values that they operate on. Each DS2 function belongs to one of the following categories:
> - Array
operates on a named aggregate collection of homogenous data
> - Bitwise Logical Operations
operates on one or more bit patterns or binary numbers at the level of their individual bits
> - Character
operates on character data and SQL expressions
> - Date and Time
operates on date and time values
> - Descriptive Statistics
operates on values that measure central tendency, variation among values, and the shape of distribution values
> - Distance
returns the geodetic distance
> - Financial
calculates financial values such as interest, periodic payments, depreciation, and prices for European options on stocks.
> - Hyperbolic
performs hyperbolic calculations such as sine, cosine, and tangent
> - Mathematical
operates on values to perform general mathematical calculations
> - Numeric
operates on numeric values
> - Probability
returns probability calculations.
> - Quantile
returns a quantile from specific distributions
> - Random Number
returns random variates from specific distributions
> - Special
operates on null values and SAS missing values, suspends execution of a program, specifies numeric informats at run time, and executes a FedSQL statement.
> - Trigonometric
operates on values to perform trigonometric calculations
> - Truncation
operates on values to limit the number of digits
> - Variable Information
operates on variables and returns names, types, lengths, informats, labels, and other variable information

Array为例，共计如下4个函数，

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.3-000.png" width=400>
</p>

点进DIM函数，也能发现所在Category。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.3-001.png" width=400>
</p>

（2）See Also
一般地，查看某函数时，文末都有一段See Also，带出类似函数。
以DIM函数为例，就把该Category下的其他类似函数链接出来。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.4.3-002.png" width=400>
</p>

#### 1.2.4.4 总结
整体来说，SAS Help and Documentation的编写还是有一定水平的，始终是最主要的SAS学习文档。需要使用者在应用过程中，不断学习总结，相信SAS水平会有更加长足的进步。此外，除了SAS Help外，剩余资源主要聚焦在人大经济论坛部分早期文档。这里可以看到，SAS整体开发社区还是比较小，后续笔者将再做整理，欢迎持续关注。
如有更新，可能会在 [https://github.com/IvanaXu/DataSAS](https://github.com/IvanaXu/DataSAS)

### 1.2.5 SAS IDE
#### 1.2.5.1 SAS-BASE
Win + R，输入sas即可调出SAS BASE，页面如下：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.1-000.png">
</p>

- 逻辑库-浏览创建SAS库管理、浏览SAS库文件（移动、复制、更名和删除）、建立非SAS文件的快捷方式；
- 输出结果-以树形结构展示提交SAS程序输出的各项结果，查看、存储、打印或删除各项结果的内容；
- 编辑器-对SAS程序语法检查、程序段的收缩和展开、可记录宏、自定快捷键；
- 日志页面-基本窗口，缺省地打开，依次记录SAS进程中各程序运行的信息，可用命令清空；

这里指出，SAS BASE相比SAS EG，语法要求更严谨、没有代码提示、需手动查看日志、不支持中文变量，对代码要求更高，在非远程连接模式下，一般我们建议使用SAS BASE。
#### 1.2.5.2 SAS-EG
SAS Enterprise Guide基本使用教程，特指远程连接模式：

（1）桌面双击打开或开始菜单中打开SAS Enterprise Guide 7.1(64-bit)；

（2）用户登录

（2.1）若提示情况如下：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-000.png" width=500>
</p>

输入用户名、密码，点击“确认”即可。

（2.2）若提示情况如下：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-001.png" width=500>
</p>

点击“是”，进入连接属性，如下。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-002.png" width=500>
</p>

（2.3）若无配置文件列表，则点击“添加”。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-003.png" width=500>
</p>

（2.4）在设置中填写名称（可自定义），远程连接服务器的地址、端口、登录的用户名、密码等。填写完毕后点击“保存”，后续步骤与已有配置文件的步骤（2.5）一致。

（2.5）若已有配置文件，则选择给定的特定文件，点击“设为活动”，将弹出用户登录界面。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-004.png" width=500>
</p>

输入给定的用户名和密码，点击“确定”。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-005.png" width=500>
</p>

点击关闭后即可。
（3）程序编写
（3.1）点击左上角文件，新建，程序。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-006.png" width=500>
</p>

（3.2）在右侧增强型编辑器区域编写代码，如下。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.2-007.png" width=500>
</p>

（3.3）代码编写完毕后，点击“运行”，或通过键盘快捷键F3或F8，运行程序。
（3.4）若需执行部分代码，选中要执行部分的代码，点击“运行”，或通过键盘快捷键F3或F8，运行程序。

#### 1.2.5.3 SAS-EM
此部分教程选自《SAS EM使用介绍——刘洋洋》
远程连接模式下，
（1）基本操作

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-000.png" width=500>
</p>

（2）新建项目
登录后界面如下图，如果已经建立过项目则选择“打开项目”，新建项目则选择“新建项目”，注意：新建项目请放置在个人目录下。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-001.png" width=500>
</p>

（3）新建逻辑库
类似sas base中的“libname”功能，加载了逻辑库，才能够提取数据。
加载逻辑库的两种方式如下图。其中逻辑库路径即分析数据的目录。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-002.png" width=500>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-003.png" width=500>
</p>

（4）创建流程图、创建数据源
创建方式如下图，新创建的流程图是空白的。
创建数据源后才可以对数据进行分析。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-004.png" width=500>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-005.png" width=500>
</p>

（5）代码编写窗口
SAS EM中有类似base的代码编写功能，在这里可以做一些数据查看和处理。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.3-006.png" width=500>
</p>

（6）建模功能
> 详见 模型策略部分。


#### 1.2.5.4 SAS-University
> [https://www.sas.com/en_us/software/university-edition/download-software.html](https://www.sas.com/en_us/software/university-edition/download-software.html)

SAS大学生版本，通过VirtualBox的轻量级（2G）SAS版本，支持Windows、MacOS、Linux，安装后启动，可在浏览器通过SAS Studio/JupyterLab编写、运行代码，非常适用于SAS学习、示例代码开发。
安装步骤：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.4-000.png">
</p>

开启页面：
输入浏览器上输入[http://0.0.0.0:10080/](http://0.0.0.0:10080/)

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.4-001.png">
</p>

JupyterLab：
与python-Jupyter一致，易保存易共享，本文后续SAS代码大部分经此工具校验调试。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.4-002.png">
</p>

未来可期！

> 特别的，由于依托Linux环境，SAS路径一般在 /opt/sasinside/SASHome/SASFoundation/9.4/sas，且SAS版本为9.4_M6：
>
> **9.4_M6的特殊性在于调整SAS中文编码字符数由2至3，因SUBSTR("堂",1,1) = SUBSTR("商",2,1) 这种偶然性而影响SAS中文判断的bug将大幅减少！**
> 
> 请多关注SAS版本修订。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.5.4-003.png" width=400>
</p>

### 1.2.6 SAS代码规范

- SAS命名规范（试行）

（1）命名由数字、英文字母、下划线组成，且由英文字母开头，不区分大小写，不使用中文命名；

（2）命名总长度不超过32个字符长度，若过长可调整部分缩写，如CRED_LIMIT转换CREDL；

（3）命名不使用常见代码关键词，如SQL、RUN、DATA等；

（4）计算逻辑，如A/B表示为A_PCT_B；

（5）特定前缀命名，如判断是否IS、统计函数类：SUM、MAX、MIN、AVG等、列表LIST、时间类：DT（具体时间）、DAY（天数）等、金额类AMT、数量类CNT、占比类PCT；

（6）特定后缀命名，如时间类（默认为账单月）：前一个月L01（LAST）、后一个月N01（NEXT），如产品类，分期产品ZDFQ、LHFQ等；

（7）特定意义命名，如银联命名习惯（CUSTR-客户）、分期MP、账龄MOB等；

（8）尽量使用英文命名，减少使用中文拼音全拼，避免使用中文拼音缩写；

**PS：部分SAS版本也是支持中文命名的，很方便也很好理解，但会无限制使命名过长；**

### 1.2.7 SAS⽚段

以下摘录部分常用SAS代码片段。

#### 
～逻辑库链接（仅作示例）

```SAS
%MACRO CONNECT_LIBNAME();
LIBNAME ODS greenplm dsn="Greenplum Wire Protocol" user="readuser" pass=readuser schema=sdm;
LIBNAME MD greenplm dsn="Greenplum Wire Protocol" user="readuser" pass=readuser schema=adm;
LIBNAME RPT greenplm dsn="Greenplum Wire Protocol" user="readuser" pass=readuser schema=rpt;
LIBNAME BSD "/data2/userhome/DEV-HOME/SAS-DEV-OUTBOUND-01/Data"
%MEND CONNECT_LIBNAME;
%CONNECT_LIBNAME();
```

#### ～格式刷

```SAS
%MACRO FMT(TB=, KEY=, DESC=, FMT_NAME=);
PROC SORT DATA = &TB. NODUPKEY OUT = INDATA;
BY &KEY. &DESC.;
QUIT;

PROC CONTENTS DATA = INDATA OUT = CONT NOPRINT;RUN;
PROC SQL NOPRINT;
SELECT TYPE INTO: KEY_TYPE FROM CONT
WHERE UPCASE(NAME) = UPCASE("&KEY.");
QUIT;

%PUT &KEY_TYPE.;
DATA &FMT_NAME.;
LENGTH LABEL $200.;
SET INDATA END = LAST;
START = COMPRESS(&KEY.);
LABEL = COMPRESS(&DESC.);
RETAIN FMTNAME "&FMT_NAME.";
%IF &KEY_TYPE. = 2 %THEN TYPE = "C";
%ELSE TYPE = "N";
;
OUTPUT;
IF LAST THEN DO;
START = "**OTHER**";
LABEL = "UNKNOWN";
HLO = "O";
OUTPUT;
END;
RUN;

PROC FORMAT CNTLIN = &FMT_NAME.;RUN;
%MEND FMT;

%FMT(TB=SASHELP.CARS, KEY=MODEL, DESC=MAKE, FMT_NAME=FMT_MODEL);
```

#### ～程序休眠

```SAS
/* 程序将休眠1h */
%MACRO CALL_SLEEP(HOURS=1);
    DATA _NULL_;
    CALL SLEEP(&HOURS., 3600);
    RUN;
%MEND;
%CALL_SLEEP();
```

#### ～条件构建宏

根据筛选条件构建如`MSRP IN(90520)`的宏变量，用于where语句。

```SAS
%MACRO COND_CRE(SRC_LIB=, SRC_TB=, TARGET_VAR=, INCOND=, OUTCOND=, MAXSIZE=1000);
%GLOBAL &OUTCOND.;

PROC SORT 
    DATA = &SRC_LIB..&SRC_TB.(WHERE = (&INCOND.))
    OUT = TEMP(KEEP = &TARGET_VAR.) NODUPKEY;
BY &TARGET_VAR.;
RUN;

PROC TRANSPOSE DATA = TEMP OUT = T_TEMP PREFIX = A;
VAR &TARGET_VAR.;
RUN;

DATA T2_TEMP;
SET T_TEMP;
ARRAY ARR1(*) A:;
LENGTH COND $&MAXSIZE..;
COND = "";
DO I = 1 TO DIM(ARR1);
    COND = COMPRESS(COND)||COMPRESS(ARR1[I])||',';
END;
COND = COMPRESS(COND);
COND = SUBSTR(COND,1,LENGTH(COND)-1);
COND = "&TARGET_VAR. IN("||COMPRESS(COND)||")";
KEEP COND;
RUN;

PROC SQL NOPRINT;
SELECT MAX(COND) INTO: &OUTCOND. FROM T2_TEMP;
QUIT;

%MEND COND_CRE;

%COND_CRE(
    SRC_LIB=SASHELP, SRC_TB=CARS, 
    TARGET_VAR=MSRP, INCOND=MSRP>90000, 
    OUTCOND=_T, MAXSIZE=1000
);

PROC PRINT DATA = SASHELP.CARS;
WHERE &_T.;
RUN;
%PUT &_T.;
/* MSRP IN(90520,94820,121770,126670,128420,192465) */
```

#### ～FTP下载/上传文件

```SAS
/* RF */
%MACRO RF(FILENAME=,OUTFILE=);
	FILENAME &OUTFILE. FTP "&FILENAME." CD="/test/"
	USER="ip1" PASS="xuyifan" HOST="127.0.0.1" PROMPT;
%MEND RF;

%RF(FILENAME=run_all.csv,OUTFILE=RM);	

/* RF, 数据读取 */
DATA TA;
%LET _EFIERR = 0;
INFILE RM
DELIMITER = "," MISSOVER DSD LRECL=32767 FIRSTOBS=1 ENCODING="UTF-8" TERMSTR=LF;
FORMAT A B $20.;
INPUT 
A $
B $
;
RUN;


/* WF */
%MACRO WF(FILENAME=,OUTFILE=);
	FILENAME &OUTFILE. FTP "&FILENAME." CD="/test/result/"
	USER="ip1" PASS="xuyifan" HOST="127.0.0.1" PROMPT;
%MEND WF;

%WF(FILENAME=TA.csv,OUTFILE=WM);

/* WF, 数据写入 */
DATA _NULL_;
SET TA;
FILE WM LRECL=32767 DLM="@" ENCODING="UTF-8";
PUT 
A
B
;
RUN;
```

#### ～月度循环

```SAS
%MACRO RCYC(ST=, ED=,);
    DATA _NULL_;
    CALL SYMPUT("DEV", INTCK("MONTH", INPUT("&ST.01", YYMMDD8.), INPUT("&ED.01", YYMMDD8.)));
    RUN;
    %PUT &DEV.;

    %DO I=0 %TO &DEV.;
        DATA _NULL_;
        CALL SYMPUT("LMON", PUT(INTNX("MONTH", INPUT("&ST.01", YYMMDD8.), &I.-1),YYMMN6.));
        CALL SYMPUT("NMON", PUT(INTNX("MONTH", INPUT("&ST.01", YYMMDD8.), &I.+0),YYMMN6.));
        CALL SYMPUT("TMON", PUT(INTNX("MONTH", INPUT("&ST.01", YYMMDD8.), &I.+1),YYMMN6.));
        RUN;
        %PUT &I. &LMON. &NMON. &TMON.;
    %END;
%MEND;
%RCYC(ST=201810, ED=201812);
/*
2
0 201809 201810 201811
1 201810 201811 201812
2 201811 201812 201901
*/
```

#### ～DO OVER

```SAS
DATA A;
INPUT X Y $ Z $ M;
CARDS;
. . . 1
1 A B 4
;
RUN;

DATA B;
SET A;
ARRAY CHAR _CHARACTER_;
ARRAY NUM _NUMERIC_;
DO OVER CHAR;
    IF CHAR EQ " " THEN CHAR = "/";
END;
DO OVER NUM;
    IF NUM EQ . THEN NUM = 0;
END;
RUN;

PROC PRINT DATA = B;
RUN;
/*
Obs	X	Y	Z	M
1	0	/	/	1
2	1	A	B	4
*/
```

#### ～格式化加强理解

```SAS
/* 简单 */
PROC FORMAT;
    VALUE RR
        LOW-50 = "(LOW,50]"
        50<-60 = "(50,60]"
        60<-HIGH = "(60,HIGH)"
    ;
RUN;

PROC FORMAT;
    VALUE $RRR
        "男" = "1"
        "女" = "2"
    ;
RUN;

PROC FREQ DATA = SASHELP.CLASS;
    TABLE HEIGHT SEX;
    FORMAT HEIGHT RR. SEX $RRR.;
RUN;

/* 复杂 */
%MACRO FMT(TABLE=, VAR=);
    PROC SUMMARY DATA = &TABLE. NWAY MISSING;
        CLASS RK_&VAR.;
        VAR &VAR.;
        OUTPUT OUT = SUMM(DROP = _TYPE_ _PREQ_) MIN = VAR_MIN MAX = VAR_MAX;
    RUN;
    PROC SORT DATA = SUMM;BY RK_&VAR.;RUN;
    DATA SUMM;
        SET SUMM;
        BY RK_&VAR.;
        RK_&VAR. = _N_;
    RUN;

    PROC SORT DATA = SUMM;BY DESCENDING RK_&VAR.;RUN;
    DATA FMT;
        SET SUMM END = LAST;
        RETAIN EEXCL "Y" SEXCL "N";
        START = PUT(VAR_MIN, 8.2);
        END = PUT(LAG(VAR_MIN), 8.2);
        IF _N_ EQ 1 THEN END = "HIGH";
        IF LAST THEN START = "LOW";
        FMTNAME = "&VAR.";
        LABEL = CATS(PUT(RK_&VAR., Z2.), "(", START, "-", END, ")");
    RUN;
    PROC FORMAT CNTLIN = FMT;RUN;
%MEND;

PROC RANK DATA = SASHELP.CLASS GROUPS = 4 OUT = RK;
    VAR HEIGHT;
    RANKS RK_HEIGHT;
RUN;

%FMT(TABLE=RK, VAR=HEIGHT);

PROC FREQ DATA = SASHELP.CLASS;
    TABLE HEIGHT;
    FORMAT HEIGHT HEIGHT.;
RUN;
```
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-000.png" width=400>
</p>

#### ～PROC SUMMARY

```SAS
PROC SUMMARY DATA=SASHELP.CLASS NWAY MISSING;
    CLASS AGE;
    VAR HEIGHT;
OUTPUT OUT = Q N= MEAN= SUM=/AUTONAME;
RUN;

PROC PRINT DATA = Q;
RUN;

/* PROC SUMMARY DATA = RES2018M3 NWAY MISSING;
    CLASS KDT IA TA MA
    ;
    VAR COM03 COM06 COM12 RCRED_LMT;
    OUTPUT OUT = SUN2018M3(DROP = _TYPE_ RENAME = (_FREQ_ = NUM)) SUM=;
RUN; */
```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-001.png" width=400>
</p>

#### ～哈希表HASH连接

```SAS
DATA TMP;
    SET SASHELP.CLASS(OBS=2);
    LIFT = _N_;
    KEEP AGE LIFT;
RUN;

DATA H_LIFT;
    SET SASHELP.CLASS;
    * 进行初始化;
    IF _N_ EQ 0 THEN SET TMP;
    IF _N_ EQ 1 THEN
        DO;
            DECLARE HASH H(DATASET: "TMP");
            H.DEFINEKEY("AGE");
            H.DEFINEDATA("LIFT");
            H.DEFINEDONE();
        END;
    * IF H.FIND() EQ 0 THEN OUTPUT;
    IF H.FIND() EQ 0 THEN H_LIFT=LIFT;
    ELSE CALL MISSING(H_A);
    * ELSE H_LIFT=0;
    DROP LIFT;
RUN;
```

#### ～排列组合

```SAS
%LET N = 3; 
PROC SQL NOPRINT;
    SELECT QUOTE(STRIP(NAME)) INTO :NAMES SEPARATED BY " "
    FROM SASHELP.CLASS(OBS = &N.);
QUIT;
%PUT &NAMES.;

%MACRO ALL_COMB;
    PROC DATASETS LIB = WORK NOLIST NODETAILS;
        DELETE APP_ALL_COMB;
    RUN;
    
    %DO I = 1 %TO &N.;
        DATA ALL_COMB;
            LENGTH VARS $488.;
            ARRAY ARR(&N.) $32 (&NAMES.);
            NCOMB = COMB(DIM(ARR), &I.);
            DO I = 1 TO NCOMB;
                CALL ALLCOMB(I, &I., OF ARR(*));

                VARS = CATX(" ",OF ARR1-ARR&I.);
                KEEP VARS;
                OUTPUT;
            END;
        RUN;
        PROC APPEND 
            BASE = APP_ALL_COMB 
            DATA = ALL_COMB;
        RUN;
    %END;
%MEND;
%ALL_COMB;

PROC PRINT DATA = APP_ALL_COMB;
RUN;
```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-002.png" width=400>
</p>

#### ～Lift提升度

```SAS
PROC SQL NOPRINT;
    SELECT MEAN(HEIGHT) INTO :MEAN FROM SASHELP.CLASS;
QUIT;

%PUT &MEAN.;
/* 62.33684 */

PROC SQL;
    CREATE TABLE LIFT AS
    SELECT 
        AGE,
        MEAN(HEIGHT) AS MEAN,
        MEAN(HEIGHT)/&MEAN. AS LIFT
    FROM SASHELP.CLASS
    GROUP BY 1
    ORDER BY 2 DESC;
QUIT;
```

#### ～PROC SQL

```SAS
PROC SQL;
    CREATE TABLE TMP1 AS
    SELECT 
        AGE,
        COUNT(*)/19 AS PCT
    FROM SASHELP.CLASS
    GROUP BY 1
    ORDER BY 2 DESC;
QUIT;

PROC SQL;
    CREATE TABLE TMP2 AS
    SELECT 
        AGE,
        COUNT(*)/19 AS PCT
    FROM SASHELP.CLASS
    GROUP BY AGE
    ORDER BY PCT DESC;
QUIT;

PROC SQL;
    CREATE TABLE TMP3 AS
    SELECT 
        AGE,
        COUNT(*)/19 AS PCT FORMAT PERCENT10.2
    FROM SASHELP.CLASS
    GROUP BY AGE
    ORDER BY PCT DESC;
QUIT;
```

#### ～SAS绘图

```SAS
PROC RANK DATA = SASHELP.CLASS GROUPS = 4 OUT = RK;
    VAR HEIGHT;
    RANKS RK_HEIGHT;
RUN;

* 修改系统默认MAXOBS
ODS GRAPHICS ON/MAXOBS=10000000; 
PROC SGPLOT DATA = RK;
    VBAR 
        RK_HEIGHT/RESPONSE = RK_HEIGHT 
        STAT = FREQ DATALABEL;
    VLINE 
        RK_HEIGHT/RESPONSE = HEIGHT 
        STAT = MEAN Y2AXIS DATALABEL DATALABELPOS = BOTTOM;
RUN;
ODS GRAPHICS OFF;

* VBAR 柱状图;
* VLINE 折线图;
* STAT 统计量;
* DATALABEL 标签;
* DATALABELPOS 标签位置;
* Y2AXIS 次坐标;
```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-003.png" width=400>
</p>

#### ～自制数据集

```SAS
DATA VARL;
    FORMAT VARS $40. VART $40. VARC $100.;
    INFILE DATALINES DELIMITER = "|";
    INPUT VARS $ VART $ VARC $;
DATALINES;
ACTIVE_MOB|BASE|POINTS=21/41
AGENCY_VISE_SUM|MEAN_6M|POINTS=O,INC=1
;

PROC PRINT DATA = VARL;
RUN;
```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-004.png" width=400>
</p>

#### ～宏变量工厂

```SAS
DATA CARS;
SET SASHELP.CARS(OBS=3);
RUN;

%MACRO M();
    DATA _NULL_;
        SET CARS NOBS = N;
        CALL SYMPUT(COMPRESS("VARS_"||_N_), COMPRESS(MSRP));
        CALL SYMPUT("NAL", N);
    RUN;
    %DO I = 1 %TO &NAL.;
        %PUT &&VARS_&I..;
    %END;
%MEND M;
%M();
/*
    36945
    23820
    26990
*/
```

#### ～判断表是否存在

```SAS
%MACRO M();
    %IF %SYSFUNC(EXIST(SASHELP.CARS))
            %THEN %PUT SASHELP.CARS EXIST;
    %ELSE %PUT SASHELP.CARS NOT EXIST;
%MEND M;
%M();
/* SASHELP.CARS EXIST */
```

#### ～KS计算

```SAS
DATA TA;
SET SASHELP.CARS;
IF CYLINDERS IN (4,6) THEN K = 0;
ELSE K = 1;
KEEP MSRP K;
RUN;

PROC SORT DATA = TA;
BY MSRP;
RUN;

PROC RANK DATA = TA OUT = TB GROUPS = 5;
VAR MSRP;
RANKS GMSRP;
RUN;

PROC FREQ DATA = TB NOPRINT;
TABLE GMSRP * K/OUT = TC;
RUN;

PROC NPAR1WAY DATA = TC KS NOPRINT;
CLASS K;
VAR GMSRP;
FREQ COUNT;
OUTPUT OUT = TD;
RUN;

PROC PRINT DATA = TD(KEEP = _D_);
RUN;
/* 0.59527 */
```

#### ～批量SQL(宏)

```SAS
%MACRO PSQL(PARA1=, PARA2=,);
    SUM(CASE WHEN UPCASE(TYPE) = "&PARA1." THEN 1 ELSE 0 END) AS CNT_&PARA1.,
    SUM(CASE WHEN UPCASE(TYPE) = "&PARA1." THEN &PARA2. ELSE 0 END) AS SUM_&PARA1._&PARA2.,
    MAX(CASE WHEN UPCASE(TYPE) = "&PARA1." THEN &PARA2. ELSE 0 END) AS MAX_&PARA1._&PARA2.,
%MEND PSQL;

PROC SQL;
CREATE TABLE TEMP(DROP = _T) AS 
SELECT
    MAKE,
    %PSQL(PARA1=SUV, PARA2=MSRP)
    %PSQL(PARA1=SUV, PARA2=INVOICE)
    %PSQL(PARA1=SPORTS, PARA2=MSRP)
    %PSQL(PARA1=SPORTS, PARA2=INVOICE)
    1 AS _T
FROM SASHELP.CARS(OBS=100)
GROUP BY MAKE;
QUIT;

PROC PRINT DATA = TEMP;
RUN;
```

### 1.2.8 SAS编译

经典问题（简化自《SAS培训_PDV精华》）：
> 交易流水表EVENT月切表，数据字段：账户号XACCOUNT、交易时间INP_DAY、交易金额BILL_AMT，已根据XACCOUNT、INP_DAY排序。
> 限一个SAS DATA步，求每个账户首次达到消费金额1288的时间以及当月累计消费金额。
> 数据集示例：


```SAS
DATA EVENT;
    FORMAT XACCOUNT $8. INP_DAY BILL_AMT;
    INFILE DATALINES DELIMITER = "|";
    INPUT XACCOUNT $ INP_DAY BILL_AMT;
DATALINES;
00000001|20200101|1200
00000001|20200102|120
00000001|20200108|302
00000002|20200101|323
00000002|20200102|423
00000002|20200110|135
00000003|20200101|3232
00000003|20200101|544
00000003|20200107|423
00000003|20200109|135
;
/* 已排序 */
PROC SORT DATA = EVENT;
BY XACCOUNT INP_DAY;
RUN;

```

求解过程：

```SAS
DATA RESULT1(KEEP = XACCOUNT RAMT1 RDAT1) RESULT2;
SET EVENT(IN = E);
BY XACCOUNT;
PUT "S0:" _ALL_;

IF FIRST.XACCOUNT THEN DO;
    RAMT1 = 0;
    RAMT2 = 0;
    RAMT3 = 0;
    RDAT1 = 99999999;
    RDAT2 = 99999999;
END;

RAMT1 + BILL_AMT;

RETAIN RAMT2;
RAMT2 = SUM(RAMT2, BILL_AMT);

RAMT3 = SUM(RAMT3, BILL_AMT);

RETAIN RDAT1;
IF RAMT1 >= 1288 THEN RDAT1 = MIN(INP_DAY, RDAT1);

IF RAMT1 >= 1288 THEN RDAT2 = MIN(INP_DAY, RDAT2);

PUT "S1:" _ALL_;
IF LAST.XACCOUNT
    THEN OUTPUT RESULT1;
OUTPUT RESULT2;
RUN;
```

输出结果：

```SAS
S0:E=1 XACCOUNT=00000001 INP_DAY=20200101 BILL_AMT=1200 FIRST.XACCOUNT=1 LAST.XACCOUNT=0 RAMT1=0 RAMT2=. RAMT3=. RDAT1=. RDAT2=.
_ERROR_=0 _N_=1

S1:E=1 XACCOUNT=00000001 INP_DAY=20200101 BILL_AMT=1200 FIRST.XACCOUNT=1 LAST.XACCOUNT=0 RAMT1=1200 RAMT2=1200 RAMT3=1200
RDAT1=99999999 RDAT2=99999999 _ERROR_=0 _N_=1

S0:E=1 XACCOUNT=00000001 INP_DAY=20200102 BILL_AMT=120 FIRST.XACCOUNT=0 LAST.XACCOUNT=0 RAMT1=1200 RAMT2=1200 RAMT3=. RDAT1=99999999
RDAT2=. _ERROR_=0 _N_=2

S1:E=1 XACCOUNT=00000001 INP_DAY=20200102 BILL_AMT=120 FIRST.XACCOUNT=0 LAST.XACCOUNT=0 RAMT1=1320 RAMT2=1320 RAMT3=120
RDAT1=20200102 RDAT2=20200102 _ERROR_=0 _N_=2

S0:E=1 XACCOUNT=00000001 INP_DAY=20200108 BILL_AMT=302 FIRST.XACCOUNT=0 LAST.XACCOUNT=1 RAMT1=1320 RAMT2=1320 RAMT3=. RDAT1=20200102
RDAT2=. _ERROR_=0 _N_=3

S1:E=1 XACCOUNT=00000001 INP_DAY=20200108 BILL_AMT=302 FIRST.XACCOUNT=0 LAST.XACCOUNT=1 RAMT1=1622 RAMT2=1622 RAMT3=302
RDAT1=20200102 RDAT2=20200108 _ERROR_=0 _N_=3

```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.2.7.0-005.png" width=400>
</p>

显然，我们在RAMT1、RDAT1上把要求逻辑实现了一遍，且PUT出来了DATA执行前后`_ALL_`的变化。
这些输出意味着什么呢？在回答此问题之前，需要先了解SAS数据步的编译。
> 数据步的编译和执行过程：
> 编译过程：
> 在这个阶段，系统扫描每个语句检查它是否有语法错误。大部分语法错误导致系统无法对数据步作进一步的处理，在编译阶段将建立要创建的数据集的描述部分。
> 
> 执行阶段：
> 若数据步编译成功，就要开始执行阶段。在这个阶段对源数据文件的每一条记录都执行一次数据步，除非在程序中指明其他处理方式。在这个阶段建立数据集的数据部分。


具体来说，
（1）数据步的编译阶段：

- 即对程序进行词语和语法编译梦，检查它是否有语法错误；

如，漏掉或错拼的关键词、无效的变量名、遗漏或错误的符号、无效的选择项；

- 将程序转换为机器码，供执行阶段使用；
- 建立工作部件输入缓冲器（Input Buffer）；
- 建立工作部件PDV（程序数据列，Program Data Vector）；

用于建立SAS系统数据集，一次只处理一个观测，PDV一般格式为`[_N_|_ERROR_]`，
其中`_N_`记录DATA步执行的次数，`_ERROR_`指示出错信息，0为无错误，1为有错误；

- 建立数据集中每个变量的三个必须的属性：Name、Type、Length；
- 建立新建数据集的描述部分；

即数据集名、观测数和变量个数、变量名及其属性；

（2）数据步的执行阶段：
即创建数据集的数据部分，执行顺序：

- PDV中外部变量初始化为缺省值；
- 输入每条记录至输入缓冲器，按INPUT语句读至PDV（如有）；
- 按数据步的其他语句处理后，存入PDV；
- 在数据步结束时缺省地将PDV内容作为一条新观测写入新的数据集；
- 回到数据步的开始，使PDV中外部变量初始化为缺省值；
- 对源文件中每条记录都按上述步骤执行一次；
- 当对源文件最后一条记录执行结束后，数据步执行完成；

上述步骤中，在数据步迭代开始时外部变量初始化为缺省值，但以下几种情况不受限制：

- RETAIN语句；
- SUM语句（A + 1）；
- 数组`_TEMPORARY_`；
- FILE、INFILE语句；
- 自动生成变量，如`_ERROR_、_N_`等；

PS：上述变量自然不包含宏变量，宏变量声明后记录在`SASHELP.VMACRO`中。
如果还是觉得PDV太过抽象，可以这么理解：
> 个人认为可以把PDV想象成一排用于存放变量值的盒子。
> 每个盒子代表一个变量，提交一个DATA步后，SAS会对这个DATA步进行编译，然后执行。
> 首先，PDV是在DATA步的编译阶段生成的。（编译会进行语法检查并创建一排整齐摆放的”盒子”；）然后，在DATA步的执行阶段，根据不同语句对PDV中变量的值进行清空或更改。（将盒子清空或换上新的物品；）
> 最后，在RUN语句或者OUTPUT语句将PDV中变量的当前值输出到目标数据集中。KEEP、DROP语句或KEEP=、DROP=数据集选项会影响输出到目标数据集中变量的个数。（如果没有KEEP、DROP，将新建变量和数据集变量对应的盒子搬出到目标数据集；如果只有KEEP，则只搬KEEP指定的盒子；如果只有DROP，则不搬DROP指定的盒子；如果KEEP/DROP同时存在，则只搬KEEP-DROP后剩下的盒子）
> ——选自网络文章《SAS中关于PDV的总结》


所以，上例在我们通过`PUT _ALL_`查看PDV中所有变量后，发现：RAMT1、RAMT2实现了累加，而未RETAIN的RAMT3、RDAT2在S0时总是先被初始化，而S1每次仅计算当前所在行数据结果。至此，问题解决，有兴趣的同学可以进一步拓展SAS PROC步、宏编译。

> **在此感谢上述代码贡献者、搬运工们。**



