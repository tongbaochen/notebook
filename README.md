
# blog-tongbaochen
# linux基本用法
> - 查看文件行数
>> wc -l x.txt
> - 查看文件夹下的空文件数目
>> ls -l | awk '{ if ($5 == 0) cnt++;} END { print "个数",cnt}' 
> - 查看文件夹下的文件数目
>> ls -l | grep "^-" | wc -l
> - 往多个文件的首行中添加一行数据
>> sed -i "1i cate3_cd,sku_id,dc_id,date,predict_sale_count" ./based_result_1.csv
> - 合并多个文件
>> cat based_result/* > based_result.csv
> - 删除以cate3开头的所有行
>> cat ./based_result.csv | sed -e '/^cate3/d' > ./based_result_1.csv
> - 获取指定的列
>> cat ./top_sku_1w_rand_train.csv | cut -d $'\t' -f 2 > cate3_id.txt
> - 匹配指定列的值符合某条件的所有行
>> cat 1w_train_joint_flow_feats.csv | awk -F , '$2==9918 && $4==6 {print }' > ./9918_6_feats_2.csv
> - 字符全匹配，每行进行同样的匹配，只要该行中存在该模式，则保留
>> grep ,1098,[0-9]*,6, 1w_train_joint_flow_feats.csv > ./1098_6_feats.csv 
> - 取前n条数据
>> head -n 200 training_0318_joint_ctb_sort_no_head_cp.csv > test_200.csv 
> - 显示前几条，后几条数据
>> ls | head
>> ls | tail
> - 将文件中的所有空格和tab的组合都转化为 ,
>> sed -i "s/\t/    /g" training_0318_joint_ctb_sort_no_head_cp.csv
>> cat training_0318_joint_ctb_sort_no_head_cp.csv | sed 's/[ ][ ]*/,/g' > training_0318_joint_ctb_sort_no_head_cp_change.csv
> - sort 排序
>> cat 1w_sample.csv|sort -k5 -t ',' > 1w_sample.csv_sort.txt
> - 取文件的某几行
>> cat 1w_sample.csv_sort_no_head | sed -n '1,10000p' > 1w_training 
> -  将文件内容转换为大写
>> cat file | tr a-z A-Z > newfile
> - 转换为小写
>> cat file | tr A-Z a-z > newfile
> - 后台执行程序
>> nohup python xgb_reproduction_mmchen_2.py  1w_training 1w_dev xgb_model_flow_based_model_1 &
# hive
> - 从hdfs上下载表
>> hive -e "
SET hive.cli.print.header=TRUE;
SELECT * from tmp.1w_train_joint_flow;
 " > /home/mart_ai/chentongbao/data4/1w_train_joint_flow.csv
> - 创建表
>> CREATE TABLE tmp.1w_predict_joint_flow AS
SELECT sys.*, 
fe.f_last1d_sku_pv, 
fe.f_last7d_sku_pv,          
fe.f_last30d_sku_pv,          
fe.f_last1d_sku_uv,        
fe.f_last7d_sku_uv,
fe.f_last30d_sku_uv,            
fe.f_last1d_sku_search_num,
fe.f_last7d_sku_search_num,          
fe.f_last30d_sku_search_num,
fe.f_last30d_comment_num,
fe.f_last1d_cart_times,
fe.f_last7d_cart_times, 
fe.f_last30d_cart_times,
fe.f_last1d_sku_visits_num,
fe.f_last7d_sku_visits_num,           
fe.f_last30d_sku_visits_num,	                     
fe.f_last1d_comment_num,        
fe.f_last7d_comment_num
from (select * from app.top_sku_features_1w_predict sy) sys LEFT JOIN adm.adm_l02_sku_profile_da fe ON (sys.sku_id=fe.item_sku_id AND sys.date=fe.dt AND sys.cate3_cd=fe.item_third_cate_cd);

> - 查看表的字段
>> desc table;

> - 将本地的数据表上传到指定的hdfs表中
>> LOAD DATA LOCAL inpath './tongbao_predict_feature_all_feats.csv' overwrite INTO TABLE tmp.tongbao_predict_feature_all_feats;
> - 建表
>> DROP TABLE IF EXISTS tmp.tongbao_train_feature_all_feats;
   CREATE TABLE tmp.tongbao_train_feature_all_feats
    (dc_id string,
     sku_id string,
    7volabilityn string) ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde' WITH SERDEPROPERTIES ("separatorChar"="\t")     STORED AS TEXTFILE;
----------

**英文原文链接**：[“Hot-Warm” Architecture in Elasticsearch 5.x](https://www.elastic.co/blog/hot-warm-architecture-in-elasticsearch-5-x).


