---
layout:     post
title:   tensorflow完整demo(读取单张测试图片方法)
category:   coding
description: 
---

接触tensorflow一段时间了，官方文档给的demo也能跑，但是一直有一个问题，不管是mnist还是cifar10的例子，均是以测试集的准确率为输出，然后就完了。如何实现拿训练好的模型来测试自己找的任意图片，无论是在官方文档还是各大博客都是搜不到的。自己困扰了很久，终于搞出来了。


基本实现就是拿opencv读图，然后用resize()转化成与训练集一样的尺寸（这里是一个矩阵形式），接着用numpy.reshape转换为一维向量，然后就可以拿去feed给初始的x值了。需要注意的是，这里reshape不能用tensorflow的，因为feed不能拿tensor去赋值。


1.基本机器学习
[完整代码](https://github.com/fire717/machine-learning/blob/master/tensorflow/basic_mnist_demo.py)
````markdown
#read any size pic
z = cv2.imread("2.png",0)
z = cv2.resize(z,(28,28),interpolation = cv2.INTER_CUBIC)
z=inverse_color(z) #因为测试集都是黑底白字，所以可以加这一步反转黑白色

image = np.reshape(z,[1,784],order='C')
#cant use tf.reshape() cause its output is a tensor while cant be feed 

x2 = tf.placeholder(tf.float32, [1, 784])
y2 = tf.nn.softmax(tf.matmul(x2,W) + b)
ans = tf.argmax(y2,1)
print sess.run(ans,feed_dict={x2:image,})
````


2.mnist卷积神经网络
https://github.com/fire717/machine-learning/blob/master/tensorflow/mnist_cnn_demo.py
````markdown
z = cv2.imread("2.png",0)
z = cv2.resize(z,(28,28),interpolation = cv2.INTER_CUBIC)

image = np.reshape(z,[1,784],order='C')
#cant use tf.reshape() cause its output is a tensor while cant be feed 

y_2 = sess.run(prediction,feed_dict={xs:image,keep_prob:1})
#y2 = tf.nn.softmax(tf.matmul(h_fc1_drop,W_fc2)+b_fc2)
#ans = tf.argmax(y2,1)
print sess.run(tf.argmax(y_2,1))
````


3.cifar10 CNN

应该和mnist的差不多，暂时没时间写这个，先放在这里吧。