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
    
> - 查询一些数据表的行数
>> select count(*) from tmp.tmp_flow_fearure_da;
> - 查询限量数据
>> SELECT * from adm.adm_l02_sku_profile_da limit 100;
> - 针对item_sku_id去重 
>> SELECT count(distinct item_sku_id) from tmp.tmp_flow_fearure_da;
