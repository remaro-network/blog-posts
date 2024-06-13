# Navigating the Unknown: How to Use Uncertainties in Deep Learning Predictions to Your Advantage

Some years ago, my former manager told me a phrase that would later make sense and guide my research interests for the next years: "I do not care that your model does not know everything, as long as I know what it does not know."

Deep learning models are often overconfident and can predict outputs with high probability even for inputs with patterns different from those used during training and validation.

This overconfidence is a problem since it creates an illusion of security in the predictions. Uncertainty analysis helps developers and users understand more about the knowledge of the models.

Imagine that you are a biology professor specializing in butterflies. One day, one of your students arrives with a butterfly you have never seen before. What do you do? You will probably give your best guess as to which family that butterfly belongs to, but you will also tell your student that you need some time to research this new species and will give them a better answer when you have more knowledge.

Ideally, deep learning models should do this: give a prediction along with a warning about their knowledge—or lack thereof—about the input pattern.

In this post, we will discuss the uncertainties of deep learning models' predictions and how you can use them to your advantage.

## What causes the uncertainties in the predictions

The uncertainties in predictions, also called predictive uncertainties, can be divided into two types:[^1]
* Epistemic Uncertainty
    * This is due to the model itself and reflects its lack of knowledge. Given more data about the patterns that the model doesn't know, this uncertainty decreases.
* Aleatoric Uncertainty
    * This is the uncertainty inherent in the data itself and can be caused, for example, by noise in the sensor that captures the data. This uncertainty will not decrease even with more data.

## How to measure the epistemic uncertainty

## Taking advabtage of the uncertainties

## References 
[^1]: [Kendall, Alex, and Yarin Gal. "What uncertainties do we need in bayesian deep learning for computer vision?." Advances in neural information processing systems 30 (2017).](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://proceedings.neurips.cc/paper_files/paper/2017/file/2650d6089a6d640c5e85b2b88265dc2b-Paper.pdf)
