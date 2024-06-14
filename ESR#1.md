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


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/a6debcff-5488-4a43-af3d-17f48333608f" alt="t_T"  height="200"><br><br>


<img src="https://github.com/remaro-network/blog-posts/assets/58445878/94b28f2a-4e74-423b-8e16-496c83f45e26" alt="t_T"  height="200"><br><br>

4. Measure the uncertainty of the outputs applying some metric to the predicted values.

### Metrics for calculating the Uncertainty

* Mutual Information
* Entropy
* Variation Ratio
* Total Variance

## To keep in mind 

The metrics explained above are applied to each output predicted. It means that for segmentation models, an uncertainty value needs to be calculated for each value predicted corresponding to the different parts (or pixels) of the image. An example is in the image bellow, that shows the uncertainties as a heatmap, where warmer colors (more yellow/red -ish) means higher uncertainty.

<img src="https://github.com/remaro-network/blog-posts/assets/58445878/1695ec9c-1291-474c-add5-f2ef51002618" alt="heatmap ex"  height="200"><br><br>

For some tasks, such as in active learning, you need to compare the amount uncertainties of predictions referent to different inputs. When dealing with segmentation, that a map of uncertainties is generated, this values need to be agreated somehow. A option for aggreating the uncertainty in this case are calculate the mean value of the uncertainties corresponding to a image.

It is important to keep in mind that it could be the case that the predictions have high uncertainty only for a small region of the images, and averaging the uncertainties for the entire image could hide the importance of this high uncertainty. Another thing to observe is that predicition of different classes of objects have different levels of uncertainty, and not considering this different classes when averaging the results could also hide important information about the uncertainty of a predicted segmentation mask.


## Examples of use cases: Taking advantage of the uncertainties

Now you have an idea of how to calculate uncertainties! But what can you do with that?

Incorrectly predictions tend to have higher uncertainties than correctly predictions. This way, the uncertainties can be used for guiding the the reliability of the predictions, and they are much more reliable than the probabilities of the softmax in the last layer of classification and segmentation models. It is important to remember that different datasets and different models have different levels of uncertainty. Therefore, a study of the uncertainty should be performed for each individual case, analysing the uncertainties in teh training and validation sets, and using this values for defining aceptable uncertainty threshoulds for using during inference.

Another use case is to help in the selection of data for being labeled to later train a model. Active learning is set family of methods that have the goal of selecting a small subset in a dataset that can be used for training a dataset that achieves performance similar to the performance achieved by the entire dataset. Being able to select a smaller dataset for labeling and training the model has the huge advantage of saving time and money with the tedious task of labeling datasets. 

An example of active learning application is the reserach that we developed for selecting a small subset of images for the task of segmenting underwater pipeline images. The image bellow explains the methodology utilized in our reserach.[^3]


<div align="center">
    <img src="https://github.com/remaro-network/blog-posts/assets/58445878/9c5759c9-2d21-41ab-bb4f-69cd1d1e7d89" alt="abstract - v3 - rev5_compressed"  height="500"><br>
</div>

As can be seen in the image above, the methodology follows the following steps:
1. Select some images in the pool of unlabeled images for being labeled;
    * Initially all the images are unlabeled and the pool of labeled images is empty;
    * In the first iteration a small percentage of images is randommely selected;
    * In the next iterations the unlabeled images are selected if the corresponding predictions performed by the model trained in (3) is abobe the threshould defined in (5)
3. Train a segmentation model
4. Calculated the uncertainty of the labeled images using MC-dropout
    * Because we want to select images that contains a pattern that the model does not have enough knowladge, we use the epstemic uncertainty
    * For calculating the uncertainty of the image, we calculated the average value of the ncertainty heatmap
5. Define a threshould based of the uncerints calculated in (3)
6. Go back to step (1)


Still using active learning, we developed a reserach for adapting a model trained with sinthetic images for being applied to real images, using the least effort for labeling images. The image bellow sumarizes our work.


<div align="center">
    <img src="https://github.com/remaro-network/blog-posts/assets/58445878/bd4ec6e2-acbb-495c-8fd6-99c67f6c80c3" alt="g11901-52" height="500">
</div>


## Check our work

We hope that you are now curious to check our paper! 

In case we would like to read our papers, here is the title for the active learning paper:

> "Uncertainty Driven Active Learning for Image Segmentation in Underwater Inspection.", by Luiza Ribeiro Marnet, Yury Brodskiy, Stella Grasshof, and Andrzej Wąsowski.

And this is the title for the sim-2-real paper:
> "Bridging the Sim-to-Real GAP for Underwater Image Segmentation",  by Luiza Ribeiro Marnet, Stella Grasshof, Yury Brodskiy, and Andrzej Wąsowski.

For the second paper we also release a dataset for underwater pipeline segmentation that can be downloaded in the [link]([https://link.springer.com/chapter/10.1007/978-3-031-59057-3_5](https://zenodo.org/records/10951363))

## References 
[^1]: [Kendall, Alex, and Yarin Gal. "What uncertainties do we need in bayesian deep learning for computer vision?." Advances in neural information processing systems 30 (2017).](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://proceedings.neurips.cc/paper_files/paper/2017/file/2650d6089a6d640c5e85b2b88265dc2b-Paper.pdf)
[^2]: [Feng, Di, et al. "A review and comparative study on probabilistic object detection in autonomous driving." IEEE Transactions on Intelligent Transportation Systems 23.8 (2021): 9961-9980.](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9525313&casa_token=8RA6XUdmNUEAAAAA:t9KrRgd3RRbMIMjXQtaOP2oxw28jWgOTqn5UvqJ7EF6HrJNf7QivccRpkkw70kWhynwM2Lph&tag=1)
[^3]: [Ribeiro Marnet, Luiza, et al. "Uncertainty Driven Active Learning for Image Segmentation in Underwater Inspection." International Conference on Robotics, Computer Vision and Intelligent Systems. Cham: Springer Nature Switzerland, 2024.](https://link.springer.com/chapter/10.1007/978-3-031-59057-3_5)
