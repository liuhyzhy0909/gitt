1、为了修复词性标注将百分数的数字和百分号分开，修改了源码：C:\Users\liuhy\AppData\Local\Programs\Python\Python36\Lib\site-packages\jieba\posseg\_init_.py
将文件夹下的_init_.py,替换上述目录下的相同文件名的文件，上述路径为结巴模块安装的目录，根据各自安装路径修改
2、dict.txt文件问分词和词性标注词典，可自定义添加
3、test_new为主程序，通过修改input_string,测试


组合查询（待确认）
组合查询算法说明
输入：查询语句字符串
输出：json字符串，格式如下
{
     “att_value”:**
“att_name”:[
**,
**,
…..
]
“relat_sign”:**
“prd_type”:[
   **,
   **,
   ……
]
}
字段说明
relat_sign代表含义及表示方法
	等于：eql
	大于：gt
	小于：lt
	大于等于：gte
	小于等于：lte
prd_type代表含义及表示方法
	货币基金：1
	理财产品：2
	纯债基金：3
attr_name代表含义及表示方法
	收益率
	七日年化收益率、预期收益率、3个月涨幅：RATE
	万份收益、六个月涨幅：RATE2
	12个月涨幅：RATE3
	理财期限：PERIOD
	锁定期：LOCKPERIOD
	起购金额：MIN_AMOUNT
问题：此种输出结果是否满足要求，需要确认
查询结果
一般情况
要求查询语句至少要包含字段值和关系词，具体不同针对字段名称的可解析类型如下：
1、	收益率：
1）	字段名称、产品类型、关系词、字段值均包含，且之间不互相矛盾
2）	不包含产品类型，但可通过字段名称确定产品类型
如：不包含产品类型或是产品类型不明确，但字段名称为七日年化收益或万份收益，则可判断产品类型为货币基金；字段名称为3个月涨幅、6个月涨幅或12个月涨幅，则可判断产品类型为纯债基金；字段名称为预期收益率，则可判断产品类型为理财产品。
3）	不包含产品类型和字段名称的情形
如：大于5%，则返回所有产品类型的收益率
2、	锁定期：
1）	 字段名称、产品类型、关系词、字段值均包含，且之间不互相矛盾
2）	不包含产品类型的情形，产品类型直接返回货币基金
3、	理财期限：
1）	字段名称、产品类型、关系词、字段值均包含，且之间不互相矛盾
2）	不包含产品类型的情形，产品类型直接返回理财产品
4、	起购金额：
1）	 字段名称、产品类型、关系词、字段值均包含，且之间不互相矛盾
2）	 不包含产品类型，其他字段包含，产品类型返回所有产品类型
3）	 不包含产品类型和字段名称的情形
如：小于5万的产品，解析为产品类型为3种，字段名称解析为起购金额，具体返回结果见测试结果
测试结果
收益率
1、收益大于6%的产品
 
说明：产品类型中，1代表货币基金，2代表理财产品，3代表纯债基金
2、收益率大于6%的理财产品
 
3、收益率大于6%的货币基金
 
4、收益率大于6%的宝宝类产品
 
5、收益率大于6%的纯债基金
 
6、预期收益率大于6%的产品
 
7、七日年化收益率大于6%的产品
 
8、收益率不低于6%
 
9、收益率大于等于百分之六
暂不支持
10、大于6%的产品
 
11、大于6.2%的理财产品
 
12、大于6%的货币基金
 
13、万份收益大于1.2的产品
 
14、六个月涨幅大于6%的产品
 
15、收益率大于5%的货币基金、纯债基金
 
16、十二个月涨幅不少于5.4%的产品
 
17、12个月涨幅大于5.4%的产品
 
锁定期
1、锁定期小于1天的宝宝类产品
 
2、锁定期小于1天的货币基金
 
3、锁定期小于一天的产品
 
4、锁定期小于1天
 
理财期限
1、期限小于30天的理财产品
 
2、理财期限小于等于三十天的产品
 

起购金额
1、起购金额小于5万的理财产品
 
2、起购金额小于六万的产品
 
3、小于5万的产品
 
特殊情况（未实现）
字段名与产品类型矛盾
如：1、锁定期小于2天的理财产品
解析：锁定期只有货币基金类有，而理财产品没有，锁定期与理财产品矛盾
2、七日年化收益率大于百分之五的理财产品
解析：七日年化收益率只有货币基金有，而理财产品对应的是预期收益率
问题：是否需要增加此种类型的解析，如需增加，以哪个为准，比如以字段名为准还是以产品类型为准
字段名与字段值矛盾
如：收益率大于30天的产品
解析：30天为锁定期或理财期限的值，不是收益率的值
问题：是否需要增加此种类型的解析，如需增加，以哪个为准，比如以字段名为准还是以字段值为准
省略关系词
“大于”、“小于”等关系词缺省，是否要自动补充，如，根据字段值或字段名称补全百分数为大于等于，起购金额大于等于，锁定期小于等于，理财期限等于
问题：是否需要增加此种类型的解析，如需增加，补全逻辑如何确定
省略字段名称
“收益率”、“起购金额”等字段名称缺省，是否要自动补全，如，根据字段值补全，若字段值为整数为全部字段，如果为小数，则为收益率或者起购金额
问题：是否需要增加此种类型的解析，如需增加，补全逻辑如何确定
