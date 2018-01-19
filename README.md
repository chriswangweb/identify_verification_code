# identify_verification_code
利用 docker 快速构建基于 opencv,tensorflow,python3的验证码识别
-------

基于这位作者，原理请查看链接的文章
http://www.bugcode.cn/break_captcha.html

**经过一番郁闷的环境搭建过程，遇到各种奇葩问题（主要是 opencv），浪费了半个小时，我决定基于 docker 构建环境
这里主要是快速部署上的操作**

##1.构建环境
获得 docker 镜像
docker pull yoanlin/opencv-python3-tensorflow

测试一下环境

    # python3
    Python 3.6.2 (default, Sep 13 2017, 14:26:54)
    [GCC 4.9.2] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import cv2
    >>> cv2.__version__
    '3.2.0'
    >>>

成功输出版本号，说明环境正常

    cd 
    wget https://s3-us-west-2.amazonaws.com/mlif-example-code/solving_captchas_code_examples.zip
    apt-get install unzip
    unzip   solving_captchas_code_examples.zip code
    cd code
    pip3 install -r requirements.txt
    pip3 install --upgrade h5py
##2.实践代码
###a.利用 opencv生成单个字符的图片

    python3 extract_single_letters_from_captchas.py
文件夹extracted_letter_images下会生成以各个字符命名的图片文件夹

    # cd extracted_letter_images
    # ls
    2  4  6  8  A  C  E  G	J  L  N  Q  S  U  W  Y
    3  5  7  9  B  D  F  H	K  M  P  R  T  V  X  Z
    # cd 2
    # ls
    000001.png  000202.png	000403.png  000604.png	000805.png  001006.png
    000002.png  000203.png	000404.png  000605.png	000806.png  001007.png
    000003.png  000204.png	000405.png  000606.png	000807.png  001008.png
    000004.png  000205.png	000406.png  000607.png	000808.png  001009.png
    000005.png  000206.png	000407.png  000608.png	000809.png  001010.png

    cd ../..

###b.开始训练（风在吼马在叫，风扇在咆哮）
python3 train_model.py
有个小 waring，不影响运行，先忽略

    2018-01-19 07:29:15.745077: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX2 instructions, but these are available on your machine and could speed up CPU computations.
    2018-01-19 07:29:15.745083: W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use FMA instructions, but these are available on your machine and could speed up CPU computations.
    29058/29058 [==============================] - 25s 872us/step - loss: 0.1935 - acc: 0.9531 - val_loss: 0.0205 - val_acc: 0.9945
    Epoch 2/10
    29058/29058 [==============================] - 26s 882us/step - loss: 0.0136 - acc: 0.9963 - val_loss: 0.0176 - val_acc: 0.9940
    Epoch 3/10
    29058/29058 [==============================] - 28s 974us/step - loss: 0.0069 - acc: 0.9980 - val_loss: 0.0058 - val_acc: 0.9983
    Epoch 4/10
    29058/29058 [==============================] - 26s 894us/step - loss: 0.0039 - acc: 0.9988 - val_loss: 0.0119 - val_acc: 0.9968
    Epoch 5/10
    29058/29058 [==============================] - 24s 843us/step - loss: 0.0059 - acc: 0.9983 - val_loss: 0.0085 - val_acc: 0.9977
    Epoch 6/10
    29058/29058 [==============================] - 25s 847us/step - loss: 0.0034 - acc: 0.9990 - val_loss: 0.0068 - val_acc: 0.9978
    Epoch 7/10
    29058/29058 [==============================] - 25s 853us/step - loss: 0.0120 - acc: 0.9979 - val_loss: 0.0151 - val_acc: 0.9958
    Epoch 8/10
    29058/29058 [==============================] - 25s 854us/step - loss: 0.0028 - acc: 0.9992 - val_loss: 0.0078 - val_acc: 0.9979
    Epoch 9/10
    29058/29058 [==============================] - 25s 850us/step - loss: 0.0015 - acc: 0.9996 - val_loss: 0.0111 - val_acc: 0.9979
    Epoch 10/10
    29058/29058 [==============================] - 26s 880us/step - loss: 0.0031 - acc: 0.9992 - val_loss: 0.0320 - val_acc: 0.9951

###3.开始识别
python3 solve_captchas_with_model.py

