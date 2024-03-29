# MachineLearning

---

[toc]





## Overviews



### 我眼中的机器学习

1. 什么是机器学习

   收集足够多的线索,包括数据和特征, 去支撑我们做出相应结论也就是模型的输出了.

   4 MAJOR STEPS OF MACHINE LEARNING, 机器学习的4个主要步骤:

   1. 采集数据

      要有样本供机器增加阅历

      (我们要有足够的数据来训练我的模型并让他学习)

   2. 处理数据

      将数据处理成方便机器理解的内容:

      - 量化

        比如是文本数据, 就要用一些方法先将其转化成模型所能识别的内容, 即数字. 

        比如: TF-IDF, WORD2VEC 等

      - 预处理

        手动或者自动的过滤一些非优质数据, 或者数据类型的转化方法, 以便我们后续数学模型的计算.

        比如: 机械式的方法, 或者算法, 比如, PCA, NORMALIZATION 等

   3. 分析数据

      总结样本中的规律(应用数据)

      并应用在未知情况下

      从而得出符合已知规律的结论

      根据应用类别和需求的不同, 机器学习算法可以大致分为以下几类:

      - 根据数据标注内容:

        * Supervised Learning, 监督学习

        * Unsupervised Learning, 无监督学习
        * Semi-supervised Learning, 半监督学习

      - 根据应用类型:

        - Reinforcement Learning, 强化学习
        - Transfer Learning, 迁移学习

   4. 总结分析

      总结实验, 后期规划, 比如分析哪个线索和特征重要, 什么因素制约了你的结果.

      当我们结束了数据分析, 得到了我们想要的输出, 我们要回过来再总结以上三个步骤, 做进一步的分析. 

      比如后期中常见的分析样本中哪些特征重要或者根据已知结果规划下一步实验和潜在结论等等.

   

2. 机器学习的发展历程, timeline of machine learning

   1. 诞生期

      早期人工智能

      - 从 0 到有的过程
      - 1958年, 逻辑回归
      - 1966年, 隐马尔可夫模型 (HMM)
      - 1967年, KNN
      - 1980年, 专家系统 XCON

   2. 崛起期

      诞生了大量耳熟能详的算法, 但受制于数据普及度和算力, 制约了发展. 此时 AI 能做的不多, 只是简单的分类和回归等等.

      - 大量基础算法诞生
      - 1986年, 神经网络(反向传播)
      - 1989年, 卷积神经网络(CNN)
      - 1995年, SVN 支持向量机
      - 1997年, RNN/LSTM
      - 1998年, AdaBoost
      - 2001年, 随机森林

   3. 爆发期

      深度学习横行

      在2010年以后, 随着GPU的普及计算力成指数增长, 推动了神经网络的发展. 神经网络的各种变种, 开始大肆刷新各种竞赛的榜单, 达到各种榜单的 state of the art. 我们进入了深度学习横行的年代.

      模仿梵高画画(Generative adversarial network, 生成对抗网络), 模仿贝多芬作曲(MUSENET, 利用几个开始的和旋, 生成符合大师风格的曲子), AlphaGo(3000年围棋文明, 看不懂, 但就是打不过)

      - AI和深度学习的全面爆发
      - 2012年, AlexNet
      - 2014年, GAN, Generative adversarial network, 生成对抗网络
      - 2015年, ResNet
      - 2017年, DenseNet
      - 2018年, Cloud AutoML
      - ...

3. 现在机器学习面临的挑战和困难, current challenges in machine learning

   - 现在主要困难是哪里:

     1. 数据, DATA, ALWAYS DATA!!!

        不仅仅是数据的数量还有数据的质量(主要体现是标注, 由监督学习)

        1. 有监督学习类的算法, 需要大量的高质量标注信息

           比如前面提到的方法, 大部分都是 supervised learning 有监督学习力, 也就是说在模型的训练过程中我们极大的依赖了样本的标注信息, 也就是我们需要有数据去指导我们模型产生对应的输出. 如果你的数据标注质量不高, 模型的 performs 将会受到极大影响. 这就好比, 你学习一个新知识, 一开始的时候受到了不靠谱的指导, 你可能最终学不到有效的知识.

        2. 复杂的模型, 比如神经网络需要巨大数量的样本

           现在潮流下, 大部分模型都是深度学习类型, 也就是说模型中包含了数以百万计的参数待训练, 这也极大的增加了对于数据数量的需求. 训练一个卷积神经网络并让他达到 SOTA(最先进的技术, 最高水平), 也就是 state of the art , 往往需要上万的样本. 而这也更进一步造成了最数据样本数量的需求和对于数据样本质量的需求. 你会发现标注一个包含了数以万计样本的数据集是非常非常花费人力和时间成本. 花了半年时间标注了包含200个新闻视频数据库的标注

        3. 对于数据质量的需求, 推动了无监督学习和半(弱)监督学习算法的发展

           让我们在训练机器学习模式的时候, 不再过分依赖样本标注信息去指导模型产生对应的输出, 但是对于这类模型的训练过程中往往缺乏有效的指导, 所以模型 performs 相较于 supervised learning 有很大的落后.

     2. 缺乏主观推理能力, 缺乏主观演绎, LACK OF SUBJECTIVE DEDUCTION

        机器学习具有很强的计算能力, 但不具备主观推理能力, 不具备主动发现新任务目标的能力.

        机器学习的大框架, 约束了机器学习的计算模式

        目前所有机器学习都只能按照人类给定的框架去进行优化, 而不能像人一样跳出这个框架推理和思考. 比如, LOSS(损失函数), REWARDS(强化学习), 评估指标等等, 这就意味着, 机器只会在你规定的方面进行计算和优化, 但不会做你没有让他做的事情.

4. 总结与机遇, SUMMARY, 总结

​				机器学习就是指运用数学模型分析数据中的模式, 然后在运用这些模式去评估未知的数据, 以得到符合规律的结果.

​				除了因为算力而形成的垄断角色之外, 在机器学习上游, 还有机会:

- 提供数据或者加工数据
- 无监督和半监督算法的发展机遇

​				GAN算法生成的样本质量一般, 经不起推敲, 并不能代替真实样本, 因此也产生了机遇:

- 改进算法, 提高输出质量
- 验证样本真实性(使用GAN算法来验证)

   5. FUTURE OF MACHINE LEARNING, 将来机器学习是什么样的定位呢

      - 简单重复的工作将会被机器取代, 同时促使人们去发现更多新的机会, 也就是说更多新机会将会被发现.

        马云认为将来的机会在以下两点, AI in HAPPINESS, HEALTH

   6. WHAT MACHINE LEARNING CAN DO IN THE FUTURE?

      机器不能用来做重大决策!

      机器最合适只能用来做推荐, 做建议.

      在一些敏感的机器学习任务中, 人们不再关心 matrix, 反而关注的模型的可解释度.

      做研究应该想的是周围有什么问题, 如何从其他角度去看问题, 不要做 follower. 也不是整天想着如何做(刷)到 SOTA. 否则人和机器没有区别.

      

       

​				

​		







## FAQ









## See Also

SOTA, 行业最优, state of the art

Supervised Learning: 监督学习

Unsupervised Learning: 无监督学习

Semisupervised Learning: 半监督学习

Reinforcement Learning: 强化学习

Transfer Learning: 迁移学习

Neural Network: 神经网络

Generative Adversarial Network (GAN): 生成对抗网络



