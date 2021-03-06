# ST\_lcsSimilarity {#reference_995518 .reference}

计算基于LCSS（Longest Common Sub Sequence）算法的两条轨迹的相似度。

## 语法 {#section_r8u_lkj_1ui .section}

``` {#codeblock_kbo_rbj_jz3}
integer ST_lcsSimilarity(trajectory traj1, trajectory traj2, float8 dist, distanceUnit unit default 'M' );
integer ST_lcsSimilarity(trajectory traj1, trajectory traj2, float8 dist, interval lag, distanceUnit unit default 'M');
```

## 参数 {#section_89i_icm_i67 .section}

|参数名称|描述|
|----|--|
|traj1|轨迹对象1。|
|traj2|轨迹对象2。|
|dist|两点之间的距离容差，单位为米。|
|lag|两点之间的时间容差。|
|unit|距离单位，允许以下值： -   'M' : 米
-   'KM': 千米
-   'D'： 度，只允许空间参考为 WGS84（4326）轨迹使用

 |

## 描述 {#section_rm1_xiv_ics .section}

LCSS用于计算最大的公共子序列。用于判断两个轨迹点是否一致的条件包括空间距离和时间距离。 返回的结果是符合条件的轨迹点的数量 。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/803696/156229618750869_zh-CN.png)

上图中轨迹点1，3， 6 符合要求，返回为 3。

通常trajectory对象中会有srid值，如果trajectory对象没有srid值，则默认为4326。

## 示例 {#section_4bu_l8w_c7j .section}

``` {#codeblock_gyu_aej_5v4}
With traj AS (
Select ST_makeTrajectory('STPO INT', 'LINESTRINGZ(114.000528 33.588163 54.87 , 114.000535 33.588235 54.85 , 114.000447 33.588272 54.69 , 114.000348 33.588287 54.73 , 114.000245 33.588305 55.26 , 114.000153 33.588305 55.3)'::geom etry,
ARRAY['2010-01-01 11:30'::tim estam p, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
ST_makeTrajectory('STPO INT', 'LINESTRINGZ(114.000529 33.588163 54.87 , 114.000535 33.578235 54.85 , 114.000447 33.578272 54.69 , 114.000348 33.578287 54.73 , 114.000245 33.578305 55.26 , 114.000163 33.588305 55.3)'::geom etry, ARRAY['2010-01-01 11:29:58'::tim estam p, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSimilarity(a, b, 100) from traj;
st_lcssimilarity
-------------------
2
(1 row)

With traj AS (
Select ST_makeTrajectory('STPO INT', 'LINESTRINGZ(114.000528 33.588163 54.87 , 114.000535 33.588235 54.85 , 114.000447 33.588272 54.69 , 114.000348 33.588287 54.73 , 114.000245 33.588305 55.26 , 114.000153 33.588305 55.3)'::geom etry,
ARRAY['2010-01-01 11:30'::tim estam p, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
ST_makeTrajectory('STPO INT', 'LINESTRINGZ(114.000529 33.588163 54.87 , 114.000535 33.578235 54.85 , 114.000447 33.578272 54.69 ,
114.000348 33.578287 54.73 , 114.000245 33.578305 55.26 , 114.000163 33.588305 55.3)'::geom etry,
ARRAY['2010-01-01 11:29:58'::tim estam p, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:34:15','2010-01-01 11:34:50','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSimilarity(a, b, 100, interval '30 seconds') from traj;
st_lcssimilarity
-------------------
2
(1 row)
```

