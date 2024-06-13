# Navigating the Unknown: How to Use Uncertainties in Deep Learning Predictions to Your Advantage

Some years ago, my former manager told me a phrase that would later make sense and guide my research interests for the next years: "I do not care that your model does not know everything, as long as I know what it does not know."

Deep learning models are often overconfident and can predict outputs with high probability even for inputs with patterns different from those used during training and validation.

This overconfidence is a problem since it creates an illusion of security in the predictions. Uncertainty analysis helps developers and users understand more about the knowledge of the models.

Imagine that you are a biology professor specializing in butterflies. One day, one of your students arrives with a butterfly you have never seen before. What do you do? You will probably give your best guess as to which family that butterfly belongs to, but you will also tell your student that you need some time to research this new species and will give them a better answer when you have more knowledge.

Ideally, deep learning models should do this: give a prediction along with a warning about their knowledge—or lack thereof—about the input pattern.

In this post, we will discuss the uncertainties of deep learning models' predictions and how you can use them to your advantage.

![blog_post](https://github.com/remaro-network/blog-posts/assets/58445878/13a353e0-6078-4fcb-99e4-a9488e258bb8)


## What causes the uncertainties in the predictions

The uncertainties in predictions, also called predictive uncertainties, can be divided into two types:[^1]
* Epistemic Uncertainty
    * This is due to the model itself and reflects its lack of knowledge. Given more data about the patterns that the model doesn't know, this uncertainty decreases.
* Aleatoric Uncertainty
    * This is the uncertainty inherent in the data itself and can be caused, for example, by noise in the sensor that captures the data. This uncertainty will not decrease even with more data.

## How to measure the predictive uncertainty

The predictive uncertainty of the deep learning predictions can be estimated using different techniques, such as Monte Carlo Dropout (MC-Dropout), Deep Ensembles, Direct Modeling, and Error Propagation.

* Direct Modeling treats the uncertainty as an output of the model. In this case, the model's head contains neurons to predict the uncertainty and the loss used for training is designed for the model to learn the uncertainty. For example, the model can predict the mean and variance of the target, where the variance corresponds to the uncertainty. A negative log-likelihood loss can be used to train the model to maximize the probability that the target is drawn from a normal distribution with the predicted mean and variance. This technique is used for leveraging the aleatoric uncertainty.
* Error propagation is used for accessing both aleatoric and epistemic uncertainties. This method captures the model’s uncertainty by propagating the variance from each layer, which arises from layers such as dropout and batch normalization, to the model’s output.
* Monte Carlo dropout (MC-dropout) is used for modeling the epistemic uncertainty. During inference, MC-Dropout allows the use of the dropout layers, and the same input sample is forward passed many times. The variance of the outputs of each forwardpass is used to assess the epistemic uncertainty.
* Deep ensembles are also used for accessing the epistemic uncertainty. An input sample is passed through the ensemble, and the variance in the outputs of the neural networks that composes the ensemble is used
to evaluate the uncertainty.

## MC-Dropout for epistemic uncertainty prediction

## Taking advabtage of the uncertainties

## References 
[^1]: [Kendall, Alex, and Yarin Gal. "What uncertainties do we need in bayesian deep learning for computer vision?." Advances in neural information processing systems 30 (2017).](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://proceedings.neurips.cc/paper_files/paper/2017/file/2650d6089a6d640c5e85b2b88265dc2b-Paper.pdf)
