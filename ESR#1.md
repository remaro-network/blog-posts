# Navigating the Unknown: How to Use Uncertainties in Deep Learning Predictions to Your Advantage

Some years ago, my former manager told me a phrase that would later make sense and guide my research interests for the next years: "I do not care that your model does not know everything, as long as I know what it does not know."

Deep learning models are often overconfident and can predict outputs with high probability even for inputs with patterns different from those used during training and validation.

This overconfidence is a problem since it creates an illusion of security in the predictions. Uncertainty analysis helps developers and users understand more about the knowledge of the models.

Imagine that you are a biologist specializing in identifying butterflies. One day, a student arrives with a butterfly you have never seen before. What do you do? You could give your best guess as to which group that butterfly belongs to, but you will also tell the student that you need more time to research this unknown species and give them a better answer when you have more knowledge.

Ideally, deep learning models should do this: give a prediction along with a warning about their knowledge—or lack thereof—about the input pattern.

In this post, we will discuss the uncertainties of deep learning models' predictions and how you can use them to your advantage.

![blog_post2](https://github.com/remaro-network/blog-posts/assets/58445878/6f70a90d-d43d-49c8-b944-2208dea52810)


## What causes the uncertainties in the predictions

The uncertainties in predictions, also called predictive uncertainties, can be divided into two types:[^1]
* **Epistemic Uncertainty**
    * This is due to the model itself and reflects its lack of knowledge. Given more data about the patterns that the model doesn't know, this uncertainty decreases.
* **Aleatoric Uncertainty**
    * This is the uncertainty inherent in the data itself and can be caused, for example, by noise in the sensor that captures the data. This uncertainty will not decrease even with more data.


## How to measure the predictive uncertainty

The predictive uncertainty of deep learning predictions can be estimated using different techniques, such as Monte Carlo Dropout (MC-Dropout), Deep Ensembles, Direct Modeling, and Error Propagation[^2].

* **Direct Modeling** treats the uncertainty as an output of the model. In this case, the model's head contains neurons to predict the uncertainty, and the loss used for training is designed for the model to learn the uncertainty. For example, the model can predict the mean and variance of the output, where the variance corresponds to the uncertainty. A negative log-likelihood loss can be used to train the model to maximize the probability that the output is drawn from a normal distribution with the predicted mean and variance. This technique leverages aleatoric uncertainty.
* **Error Propagation** is used for assessing both aleatoric and epistemic uncertainties. This method captures the model’s uncertainty by propagating the variance from each layer, which arises from layers such as dropout and batch normalization, to the model’s output.
* **MC-Dropout** is used for modeling epistemic uncertainty. During inference, MC-Dropout allows the use of the dropout layers, and the same input sample is forward-passed many times. The variance of the outputs of each forward pass is used to assess epistemic uncertainty.
* **Deep Ensembles** are also used for assessing epistemic uncertainty. An input sample is passed through the ensemble, and the variance in the outputs of the neural networks that compose the ensemble is used to evaluate the uncertainty.


## MC-Dropout for epistemic uncertainty prediction

MC-Dropout is a widely used method due its easy implementation in models that contain dropout layers. In the next paragraphs we will give more explanation on how it works and how it can be implemented.

Usually, dropout layers are used for preventing overfitting during training. Nevertheless, if this layers are allowed during inference, the uncertainty of the predictions can be assessed. Because the dropout layers randomly drops neuros of the model under a certain probability, each time that **the same input** is forward passed through the model a possibly different result is obtained. If the model is well trained and has enough knowledge about the input pattern, the outputs are going to be equal or numerically very close to each other. In case the model does not have enough knowledge about the input pattern, the outputs of each forward pass of the same input are going to result in very different outputs.

For calculating the uncertainty, the following steps are performed:
1. Allow the dropout layers;
2. Fowardpass the same input many times (_T_ times) through the deep learning model;

<img src="https://github.com/remaro-network/blog-posts/assets/58445878/210b0370-e2b9-4c97-b2e3-8150af5232ed" alt="t1"  height="200"><br><br>


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/f4897ecb-a82f-4339-9b3c-85867a2bbfcb" alt="t2"  height="200"><br><br>


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/710793fd-45d5-4a43-8710-c8bcf4c622b8" alt="3"  height="200"><br><br>


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/ebb27b3b-5163-4774-a677-64b8e6e75fc5" alt="4"  height="200"><br><br>


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/0f1d57a0-af3b-4def-b49b-999df15e3c93" alt="5"  height="200"><br><br>


4. Measure the uncertainty of the outputs applying some metric to the predicted values.

### Metrics for calculating the Uncertainty

* Mutual Information
* Entropy
* Variation Ratio
* Total Variance

## Details to keep in mind 


## Taking advantage of the uncertainties

## References 
[^1]: [Kendall, Alex, and Yarin Gal. "What uncertainties do we need in bayesian deep learning for computer vision?." Advances in neural information processing systems 30 (2017).](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://proceedings.neurips.cc/paper_files/paper/2017/file/2650d6089a6d640c5e85b2b88265dc2b-Paper.pdf)
[^2]: [Feng, Di, et al. "A review and comparative study on probabilistic object detection in autonomous driving." IEEE Transactions on Intelligent Transportation Systems 23.8 (2021): 9961-9980.](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9525313&casa_token=8RA6XUdmNUEAAAAA:t9KrRgd3RRbMIMjXQtaOP2oxw28jWgOTqn5UvqJ7EF6HrJNf7QivccRpkkw70kWhynwM2Lph&tag=1)
