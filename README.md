# 基于异质性因果推断的精准营销策略
## 项目概述
本项目利用因果推断中的元学习器（Meta-Learners）对营销优惠的个体处理效应（CATE）进行估计，旨在识别不同用户对不同优惠策略（折扣、买一送一）的敏感程度，从而制定个性化的精准营销策略，提升营销投资回报率（ROI）。
## 数据来源
本数据来自于kaggle中的Hillstrom数据集，具体链接为：https://www.kaggle.com/datasets/davinwijaya/customer-retention
## 分析流程
### 数据探索性分析（EDA）
1.检查缺失值、异常值
2.分析不同策略下的转化率、不同渠道/地区的转化提升
3.可视化购买率与用户行为的关系
### 数据预处理
1.对 zip_code、channel 进行独热编码
2.对 history 取对数变换
3.将 offer 映射为数值：0（No Offer）、1（Buy One Get One）、2（Discount）
### 因果推断建模
1.划分训练集（80%）和测试集（20%）,分层采样保证各类别比例一致
2.使用T-Learner估计CATE,基模型初始选择RandomForest,后续尝试使用XGBoost（效果更优）
3.超参数调优：网格搜索 + 加权综合得分（AUUC）
4.尝试使用R学习器估计CATE并使用参数网格进行参数调优，排序能力不然T学习器
5.最终最终 T-Learner,在测试集上的 AUUC：Discount策略auuc：0.7465，BOGO策略auuc:0.8834
### 模型导出
使用最佳参数训练全量数据模型，并保存为 t_learner_model.pkl
## 主要结论
营销策略效果：Discount（折扣）的平均转化率提升约7.6%，BOGO（买一送一）提升约4.5%。
异质性显著：不同用户对不同优惠的响应差异很大，模型能够有效识别高敏感人群。
模型表现：T-Learner + XGBoost 取得了优秀的排序能力,可支持实际业务部署。
业务建议：对模型预测 CATE 较高的用户优先推送对应优惠，预计可显著提升转化率。
## 环境要求
不要使用最新版的python环境，causml不支持3.13.9，建议使用python3.11