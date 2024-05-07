---
{"title":"基于tensorflow的人脸识别系统","comments":true,"tags":["人脸识别","python","个人项目"],"categories":"人脸识别","description":"该项目为基于TensorFlow深度学习的人脸识别系统。采用大学班级86*1000的数据集，环境基于ubuntu16.04+Anaconda4.2.0+python3.5+opencv2搭建","date":"2018-12-30 13:14:22","updated":"2024-04-28T23:10:58.603+08:00","type":"个人项目","author":"fun","publish":true,"dg-publish":true,"date created":"2024-04-28 10:27:09","permalink":"/01 工作/项目整理/基于tensorflow的人脸识别系统/","dgPassFrontmatter":true,"created":"2024-04-28T22:27:09.298+08:00"}
---


#### 环境搭建

ubuntu16.04+Anaconda4.2.0+python3.5+opencv2的环境搭建

#### 代码分析

##### 检验导包问题的代码

```python
import tensorflow as tf
# 创建一个常量 op, 产生一个 1x2 矩阵. 这个 op 被作为一个节点
# 加到默认图中.
# 构造器的返回值代表该常量 op 的返回值.
##matrix1 = tf.constant([[3., 3.]])
# 创建另外一个常量 op, 产生一个 2x1 矩阵.
##matrix2 = tf.constant([[2.],[2.]])
# 创建一个矩阵乘法 matmul op , 把 'matrix1' 和 'matrix2' 作为输入.
# 返回值 'product' 代表矩阵乘法的结果.
##product = tf.matmul(matrix1,matrix2)
from skimage import io, transform
import glob
import os
import time    
import tensorflow as tf
import numpy as np  
```



##### **将训练集下的图片resize一下，伴随着打开每一个子文件夹，我们要为其设置一个labels，一个文件夹的1000应一个label，共68个abel，这是后面检验acc的唯一指标，即是否能把测试集的照片通过我们的网络输出到指定出口得到正确的label。**

```python
path=''
#将所有的图片resize成100*100
w=128
h=128
c=3
#读取图片
def read_img(path):
    cate=[path+'/'+x for x in os.listdir(path) if os.path.isdir(path+'/'+x)]
    imgs=[]
    labels=[]
    for idx,folder in enumerate(cate):
        for im in glob.glob(folder+'/*.png'):
            print('reading the images:%s'%(im))
            img=io.imread(im)
            img=transform.resize(img,(w,h,c))
            imgs.append(img)
            labels.append(idx)
    return np.asarray(imgs,np.float32),np.asarray(labels,np.int32) 
data,label=read_img(path)
# 将所有数据分为训练集和验证集
ratio = 0.95#训练集占比
s = np.int ( num_example * ratio )
x_train = data[:s]
y_train = label[:s]
x_val = data[s:]
y_val = label[s:]
# -----------------构建网络----------------------
# 占位符
x = tf.placeholder ( tf.float32, shape=[None, w, h, c], name='x' )
y_ = tf.placeholder ( tf.int32, shape=[None, ], name='y_' )


def CNNlayer():
    # 第一个卷积层（128——>64)
    conv1 = tf.layers.conv2d (
        inputs=x,
        filters=32,
        kernel_size=[5, 5],
        padding="same",
        activation=tf.nn.relu,        kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ) )
    pool1 = tf.layers.max_pooling2d ( inputs=conv1, pool_size=[2, 2], strides=2 )
    # 第二个卷积层(64->32)
    conv2 = tf.layers.conv2d (
        inputs=pool1,
        filters=64,
        kernel_size=[5, 5],
        padding="same",
        activation=tf.nn.relu,        kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ) )
    pool2 = tf.layers.max_pooling2d ( inputs=conv2, pool_size=[2, 2], strides=2 )
    # 第三个卷积层(32->16)
    conv3 = tf.layers.conv2d (
        inputs=pool2,
        filters=128,
        kernel_size=[3, 3],
        padding="same",
        activation=tf.nn.relu,        kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ) )
    pool3 = tf.layers.max_pooling2d ( inputs=conv3, pool_size=[2, 2], strides=2 )
    # 第四个卷积层(16->8)
    conv4 = tf.layers.conv2d (
        inputs=pool3,
        filters=128,
        kernel_size=[3, 3],
        padding="same",
        activation=tf.nn.relu,      kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ) )
    pool4 = tf.layers.max_pooling2d ( inputs=conv4, pool_size=[2, 2], strides=2 )
    re1 = tf.reshape ( pool4, [-1, 8 * 8 * 128] )
    # 全连接层
    dense1 = tf.layers.dense ( inputs=re1,
                               units=1024,
                               activation=tf.nn.relu,                               kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ),                               kernel_regularizer=tf.contrib.layers.l2_regularizer ( 0.003 ) )
    dense2 = tf.layers.dense ( inputs=dense1,
                              units=512,
                              activation=tf.nn.relu,                               
kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ),                               kernel_regularizer=tf.contrib.layers.l2_regularizer ( 0.003 ) )
    logits = tf.layers.dense ( inputs=dense2,
                               units=68,
                               activation=None,                               kernel_initializer=tf.truncated_normal_initializer ( stddev=0.01 ),
                               kernel_regularizer=tf.contrib.layers.l2_regularizer ( 0.003 ) )
    return logits
# ----------网络结束---------------------------
# 定义一个函数，按批次取数据
def minibatches(inputs=None, targets=None, batch_size=None, shuffle=False):
    assert len ( inputs ) == len ( targets )
    if shuffle:
        indices = np.arange ( len ( inputs ) )
        np.random.shuffle ( indices )
    for start_idx in range ( 0, len ( inputs ) - batch_size + 1, batch_size ):
        if shuffle:
            excerpt = indices[start_idx:start_idx + batch_size]
        else:
            excerpt = slice ( start_idx, start_idx + batch_size )
        yield inputs[excerpt], targets[excerpt]
# 训练和测试数据，可将n_epoch设置更大一些
saver = tf.train.Saver ( max_to_keep=3 )
max_acc = 0
f = open ( 'ckpt1/acc.txt', 'w' )
n_epoch = 10
batch_size = 64
sess = tf.InteractiveSession ( )
sess.run ( tf.global_variables_initializer ( ) )
for epoch in range ( n_epoch ):
    start_time = time.time ( )
    # training
    train_loss, train_acc, n_batch = 0, 0, 0
    for x_train_a, y_train_a in minibatches ( x_train, y_train, batch_size, shuffle=True ):
        _, err, ac = sess.run ( [train_op, loss, acc], feed_dict={x: x_train_a, y_: y_train_a} )
        train_loss += err;
        train_acc += ac;
        n_batch += 1
    print ( "   train loss: %f" % (train_loss / n_batch) )
    print ( "   train acc: %f" % (train_acc / n_batch) )

    # validation
    val_loss, val_acc, n_batch = 0, 0, 0
    for x_val_a, y_val_a in minibatches ( x_val, y_val, batch_size, shuffle=False ):
        err, ac = sess.run ( [loss, acc], feed_dict={x: x_val_a, y_: y_val_a} )
        val_loss += err;
        val_acc += ac;
        n_batch += 1
    print ( "   validation loss: %f" % (val_loss / n_batch) )
    print ( "   validation acc: %f" % (val_acc / n_batch) )
    f.write ( str ( epoch + 1 ) + ', val_acc: ' + str ( val_acc ) + '\n' )
    if val_acc > max_acc:
        max_acc = val_acc
        saver.save ( sess, 'ckpt1/faces.ckpt', global_step=epoch + 1 )
f.close ( )

detector = dlib.get_frontal_face_detector ( )
  # 获取人脸分类器
ID = (1511346,1610731,1610763,1610260,1611407,1611408,      1611409,1611412,1611413,1611415,1611417,1611418,     1611419,1611420,1611421,1611424,1611425,1611426,      1611427,1611430,1611431,1611433,1611434,1611436,      1611437,1611438,1611440,1611444,1611446,1611447,      1611449,1611450,1611451,1511453,1611455,1611458,     1611459,1611460,1611461,1611462,1611470,1611471,    1611472,1611472,1611476,1611478,1611480,1611482,  1611483,1611486,1611487,1611488,1611490,1611491, 1611492,1611493,1611494,1613371,1613376,1613378,  1613550,1711459 )
#两个操作是拿到dlib的人脸分类器（相当于dlib的训练代码跑完的结果存下的参数变量结构等东西），然后建个数组当输出和ID的映射
#最终交互检验：
user = input ( "图片（G）还是摄像头（V）:" )
if user == "G":
    path = input ( "图片路径名是：" )
    img = cv2.imread ( path )
    dets = detector ( img, 1 )
    print ( "Number of faces detected: {}".format ( len ( dets ) ) )
    for index, face in enumerate ( dets ):
        print (
            'face {}; left {}; top {}; right {}; bottom {}'.format ( index, face.left ( ), face.top ( ), face.right ( ),                                                                    face.bottom ( ) ) )
        left = face.left ( )
        top = face.top ( )
        right = face.right ( )
        bottom = face.bottom ( )
        cv2.rectangle ( img, (left, top), (right, bottom), (0, 255, 0), 3 )
        io.imsave ( 'temp.png', img )
        img1 = io.imread ( 'temp.png' )
        img1 = transform.resize ( img1, (w, h, c) )
        cv2.imshow ( 'image', img1 )
        img1 = img[top:bottom, left:right]
        img1 = transform.resize ( img1, (w, h, c) )
        # cv2.imshow('image1',img)
        res = sess.run ( predict, feed_dict={x: [img1]} )
        print ( ID[res[0]] )
    if len ( dets ) == 0:
        img = transform.resize ( img, (w, h, c) )
        res = sess.run ( predict, feed_dict={x: [img]} )
        print ( ID[res[0]] )
        cv2.waitKey ( 0 )
        cv2.destroyAllWindows ( )
    cv2.waitKey ( 0 )
    cv2.destroyAllWindows ( )
else:
    # 打开摄像头
    cap = cv2.VideoCapture ( 0 )
    # 视屏封装格式
    while True:
        ret, frame = cap.read ( )
        gray = cv2.cvtColor ( frame, cv2.COLOR_BGR2GRAY )
        cv2.imshow ( 'frame', frame )
        # 抓取图像，s画人脸框，q结束识别
        if cv2.waitKey ( 1 ) & 0xFF == ord ( 's' ):
            cv2.imwrite ( 'now.png', frame )
            img = cv2.imread ( "now.png" )
            dets = detector ( img, 1 )
            print ( "Number of faces detected: {}".format ( len ( dets ) ) )
            for index, face in enumerate ( dets ):
                print ( 'face {}; left {}; top {}; right {}; bottom {}'.format ( index,                                                                                 face.left ( ), face.top ( ),                                                                                 face.right ( ), face.bottom ( ) ) )
                left = face.left ( )
                top = face.top ( )
                right = face.right ( )
                bottom = face.bottom ( )
                img = img[top:bottom, left:right]
            # img=io.imread('image/now.png')
            img = transform.resize ( img, (w, h, c) )
            res = sess.run ( predict, feed_dict={x: [img]} )
            print ( ID[res[0]] )
        # 退出

```



#### 实验结果分析

##### 最终的训练的结果：

![](https://pic.superbed.cn/item/5db91f90bd461d945a546d5b.jpg)

##### 根据我自己的数据化的数据图：

![](https://pic.superbed.cn/item/5db91fb3bd461d945a5476e3.jpg)

##### 识别过程：（拿我自己的照片做的测试）：

![image.png](https://i.loli.net/2019/10/30/g7GiJwR8lXVBT5a.png)