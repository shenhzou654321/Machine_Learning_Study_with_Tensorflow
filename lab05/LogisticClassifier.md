#Lab05. Implement Logistic Classification with TensorFlow - (1)

![picture](picture.PNG)

    import tensorflow as tf
   
    tf.set_random_seed(777)  # for reproducibility

    x_data = [[1, 2], [2, 3], [3, 1], [4, 3], [5, 3], [6, 2]]
    y_data = [[0], [0], [0], [1], [1], [1]]

    # placeholders ofr a tensor that will be always fed
    X = tf.placeholder(tf.float32, shape=[None, 2])
    Y = tf.placeholder(tf.float32, shape=[None, 1])

    W = tf.Variable(tf.random_normal([2, 1]), name='weight')
    b = tf.Variable(tf.random_normal([1]), name='bias')

    # Hypothesis using sigmoid: tf.div(1., 1.+ tf,exp(tf.matmul(X, W))
    hypothesis = tf.sigmoid(tf.matmul(X, W) + b)

    # cost/loss function
    cost = -tf.reduce_mean(Y * tf.log(hypothesis) + (1 - Y) * tf.log(1 - hypothesis))

    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)

    # Accuracy computation
    # True if hypothesis>0.5 else False
    # predicted: 0.5를 기준으로 참과 거짓으로 나눔
    # Accuracy: 참과 거짓 예측이 얼마나 맞았는지 정확도 계산
    predicted = tf.cast(hypothesis > 0.5, dtype=tf.float32)
    accuracy = tf.reduce_mean(tf.cast(tf.equal(predicted, Y), dtype=tf.float32))

    # Launch graph
    with tf.Session() as sess:
        # Initialize TensorFlow variables
        sess.run(tf.global_variables_initializer())

        for step in range(10001):
            cost_val, _ = sess.run([cost, train], feed_dict={X: x_data, Y: y_data})
            if step % 200 == 0:
                print(step, cost_val)

        # Accuracy
        # 주어진 X와 Y를 가지고 훈련한 모델의 정확도 평가
        h, c, a = sess.run([hypothesis, predicted, accuracy], feed_dict={X: x_data, Y: y_data})
        print("\nHypothesis: ", h, "\nCorrect (Y): ", c, "\nAccuracy: ", a)
        
[return]

0 1.7307833

200 0.5715119

400 0.5074139

. . .

9600 0.15413196

9800 0.15177831

10000 0.14949562

Hypothesis:  [[0.03074029]

 [0.15884677]
 
 [0.30486736]
 
 [0.78138196]
 
 [0.93957496]
 
 [0.9801688 ]] 
 
Correct (Y):  [[0.]

 [0.]
 
 [0.]
 
 [1.]
 
 [1.]
 
 [1.]] 
 
Accuracy:  1.0
