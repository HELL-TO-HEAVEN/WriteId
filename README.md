# WriteId
书写者识别任务

2019.5.23
1. 完成数据的处理工作，每个训练样本为100个RHS
2. 完成Data10基本框架，测试集精度达83%，训练集达92%：
  - 6层LSTM堆叠
  - batch_size=800, Adam lr=0.001, epochs=60
3. TODO:
  - 精度提升，参数初始化等
 
2019.5.24
1. 完成测试脚本，问题：单个 100RHS 测试准确率低（由于分类顺序的问题，已解决，单个80%左右），TODO：多个RHS共同判断 5-10个RHS可以达到100%精度
2. 测试不同层数
  - 6层，batch_size=1000, 70epochs，训练集精度：97.87%，测试集85.85%，模型rnn6,Dropout=0.5
  - 8层，batch_size=800，基本学不到东西，可能由于层数太多，梯度消失
  
2019.5.27
1. 100分类任务：
  双向LSTM，3层，隐藏层400(x2)，精度可以得到有效提升
  batchsize=2000，Adam加入权重衰减0.0005

2019.5.28
1. 数据清洗：
  在每个人生成RHS时，若样本中笔画数仅为1且笔画位置仅为1，即该汉字只录入了一个点，舍弃。已完成。
2. 可以考虑对数据文件生成索引文件，否则每次数据读取耗时较多（但也无所谓...）
3. 重新对清新后数据和调整数据后数据进行训练，记录参数
  - Task10: 60epochs
    - 每个人样本的序列长度：100，模型保存名rnn5.pkl(被误删了TAT)，曲线图：10-5layer-1000ba.jpg
    - 5层单向，隐藏层=800，batch_size=1000，训练样本数1000，测试600，Adam学习率0.001，权重衰减0.0005（在后期降低学习率）
    - 准确率：训练：89.01%，测试 81.30%， Loss：训练：0.2887，测试：0.4891
    
    70epochs
    - 每个人样本的序列长度：100，模型保存名rnn6.pkl，曲线图：10-6layer-1000ba.jpg
    - 6层单向，隐藏层=800，batch_size=1000，训练样本数1000，测试600，Adam学习率0.001，权重衰减0.0005（在后期降低学习率）
    - 准确率：训练：90.57%，测试 82.93%， Loss：训练：0.2574，测试：0.4415
    
    测试效果：
    - 2018310898与2018310874是比较容易分错的，是否可以对错误样本做可视化分析？？？#TODO，其他类别还行，取10个基本能保证90%的正确率。
    
    对于2018310898的测试样本的判断结果: 应该为8，而5占大多数，判断错误
    ```
    tensor([5, 5, 8, 5, 8, 8, 5, 8, 5, 5, 8, 8, 5, 5, 5, 5, 8, 2, 5, 5, 8, 2, 8, 5,
        5, 8, 5, 8, 5, 5, 8, 5, 8, 5, 8, 8, 8, 8, 5, 8, 8, 5, 5, 8, 5, 8, 8, 2,
        8, 5, 5, 5, 5, 8, 5, 8, 8, 5, 5, 8, 8, 5, 8, 5, 5, 5, 8, 5, 2, 8, 8, 5,
        5, 5, 5, 8, 8, 5, 8, 2, 5, 5, 8, 8, 8, 8, 8, 5, 5, 5, 5, 5, 5, 5, 5, 5,
        8, 5, 8, 8], device='cuda:0')
    5
    ```
    
    对于2018310874的测试样本的判断结果:5依然占大多数，判断正确
    ```
    tensor([5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 8, 8, 5, 5, 5,
        5, 5, 8, 5, 5, 5, 5, 5, 5, 5, 5, 8, 5, 5, 5, 5, 5, 5, 5, 5, 8, 5, 5, 8,
        5, 5, 5, 8, 5, 8, 5, 6, 5, 8, 5, 5, 5, 5, 5, 8, 5, 5, 8, 5, 5, 8, 2, 5,
        5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 8, 5, 5, 5, 8, 5, 5, 5, 5, 5, 5,
        5, 5, 5, 8], device='cuda:0')
    5
    ```
    60epochs
    - 每个人样本的序列长度：100，模型保存名10rnn1-bi.pkl，曲线图：10-1layer-bi.jpg
    - 1层双向，隐藏层=400，batch_size=1000，训练样本数1000，测试600，Adam学习率0.001，权重衰减0.0005（在后期降低学习率）
    - 准确率：训练：95.12%，测试 84.21%， Loss：训练：0.1463，测试：0.4825
    - 过拟合严重，加入防止过拟合
    测试效果：
    与单向6层相同，判断8样本时容易判断为5！
    
    对1层双向加0.5的Dropout，60epochs
    - 准确率：训练：95.49%，测试84.73%， Loss：训练：0.1287，测试：0.50
    提升不大？？
    - 考虑对类别5和8增加样本？
    
    对5和8两个类别增加样本，训练样本数为1600
     - 双向6层训练
    
  - Task100：60epochs
    - 每个人样本的序列长度：100，模型保存名100rnn3.pkl，曲线图：100-1.jpg
    - 3层双向，隐藏层=400，batch_size=1000，训练样本数1500，测试600，Adam学习率0.001，权重衰减0.0005（在后期降低学习率）
    - 准确率：训练：95.98%，测试：91.70%， Loss：训练：0.1213，测试：0.2532
    
    测试效果：取十个样本取多数，93%正确率，即判断有7个人错误
    错误分类
     ```
    (array([100, 101, 102, 103, 104, 105, 106]),)
    [2018311127 2018311146 2018312459 2018312470 2018312476 2018312481 2018312484]
    [2018211051 2018211080 2018210461 2018310898 2018310907 2018210461 2018310927]
    ```
    TAT发现是因为一共有107个类别，只做了100个人的分类，没看清题目和数据的后果...再训一次咯，证明前100个人均分类正确了！！
    

 
