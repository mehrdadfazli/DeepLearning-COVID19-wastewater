[![arxiv](https://img.shields.io/badge/arXiv-2010.16052-red.svg)](https://arxiv.org/abs/2212.08798)
[![researchgate](https://img.shields.io/badge/ResearchGate-COVID--19__DL-9cf)](https://www.researchgate.net/publication/366569731_Leveraging_Wastewater_Monitoring_for_COVID-19_Forecasting_in_the_US_a_Deep_Learning_study?channel=doi&linkId=63a7359ac3c99660eb9ff2f2&showFulltext=true) [![IEEE](https://img.shields.io/badge/IEEE-COVID_DeepLearning-blue)](https://ieeexplore.ieee.org/abstract/document/10386828)

Referenced paper : [Leveraging Deep Learning to Improve COVID-19 Forecasting Using Wastewater Viral Load](https://ieeexplore.ieee.org/abstract/document/10386828)

Notebook to follow and reproduce the results: [DeepLearning-COVID-wastewater.ipynb](DeepLearning-COVID-wastewater.ipynb)

A list of all dependencies: [requirement.txt](requirement.txt).

Leveraging Wastewater Monitoring for COVID-19 Forecasting in the US: a Deep Learning study
==========================================================================================

Study Summary
-------------
COVID-19 forecasting has been proven a challenging task. Many researchers have turned to wastewater viral load as a pooled sample and an early indicator of an outbreak.
We propose the use of deep learning to automatically learn the relationship between daily confirmed cases and viral load data. We trained one Deep Temporal Convolutional Networks (DeepTCN) and one Temporal Fusion Transformer (TFT) model to build a global forecasting model. We supplemented the daily confirmed cases data with viral load data and other socio-economic factors as covariates and compared the models' performances. Our results suggest that TFT outperforms DeepTCN and learns a better association between viral load and daily cases. We showed that viral load could improve the MAE in TFT by more than ~2 units and in DeepTCN by 4 units. Similar improvements were also observed in other metrics. Moreover, viral load is shown to be the second most crucial input, following the containment and health index. Our results reveal the feasibility of training a location-agnostic deep learning model that can capture the dynamics of infection diffusion given appropriate covariates including, wastewater viral load data.

Proposed Method:
---------------

Data
----

We use [Biobot's Nationwide Wastewater Monitoring Network](https://github.com/biobotanalytics/covid19-wastewater-data) for both daily cases and wastewater viral load data. We also used [Oxford Covid-19 Government Response Tracker](https://github.com/OxCGRT/covid-policy-tracker) as socio-economic covariates to further investigate the role of these factors in COVID-19 forecasting. The data after the preprocessing for two sample counties are shown below. We used 11 counties as training data and 2 counties as a holdout set to assess the generalizability of the models.

*Dauphin county, PA*

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/Dauphin%20County%2C%20PA-data.png" width=700>

*Jefferson county, KY*

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/Jefferson%20County%2C%20KY-data.png" width=700>

DeepTCN
-------

DeepTCN is an extension of the original Temporal Convolutional Networks (TCN) which allows for probabilistic and multi-horizon forecasting. DeepTCN also allows for training with covariates. TCN extends the idea of Convolutional Neural Networks (CNN) to 1-D sequential data by defining 1-D filters and sliding them over the sequence data to convolve it. It the context of time series the filters have to be applied to the past data only (causal convolution). Additionally TCN makes use of dilution to increase its receptive field and capture long-range patterns. Below is an illustration of a causal convolution for time series data.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/TCN.png" width="500"/>

The structure of the DeepTCN that we used is also shown below.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/TCN-block-nofuture-covar.png" width="600">

TFT
---

Temporal Fusion Transformer (TFT) is tranformer-based model for time series forecasting. TFT is very similar to the original transformer model and with an encoder-decoder structure. The variable selection network of the TFT enables it to learn how to distribute its attention to the different inputs using a weighted average mechanism. Once the model is trained these weights can represent the importance of the input variables. This is a rare case of some transparency for a deep learning model. The structure of a TFT is as follows.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/TFT.png" width="700">

Results
-------

We start by introducing our three selected metrics, mean absolute error (MAE), symmetric mean absolute percentage error (SMAPE), and coefficient of variation (CV):

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/metrics.png" width="300">

We train two TFT, one with *viral load* and one without *viral load*; and two DeepTCN, one with *viral load* and one without *viral load*. The *look-back window* and *forecast horizon* are set to 30 and 10 days respectively. The performance of the models are shown in the table below.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/evals.png" width="450">

TFT outperforms DeepTCN by a considerable margin. We can also see that *viral load* improves the accuracy of both models. Another observation is that the models generalized well to the holdout counties. Also, to visualize the impact of *viral load*, we plot the two TFT models (with and without viral load) for 4 counties below. One can confirm that *viral load* improves the accuracy of TFT.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/predict_train_tft_4c.png" width="500">

The predictions for all 13 counties for the two main models are also shown below. The last two counties are the holdout counties.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/backtest_ALL_tft_&_tcn_0.8_10days.png" width="1000">

Finally, we can obtain the feature importance of our TFT model. The feature importance is derived from the variable selection for encoder and decoder units separately.

<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/TFT_Encoder_importance.png" width="500">
<img src="https://github.com/mehrdadfazli/DeepLearning-COVID19-wastewater/blob/main/figs/TFT_Decoder_importance.png" width="500">

*Viral load* is the close second important inpute behind the *containment and health index*. In the decoder variable importance, *stringency index* dominates other variables. It is noteworthy that *stringency index* is *containment and health index* excluding *testing policy* and *contact tracing*. Therefore, we can claim the most important predictor of COVID-19 daily cases is the containment policies by local health officials.

---
Cite us:
---
```
@misc{fazli2022leveraging,
      title={Leveraging Wastewater Monitoring for COVID-19 Forecasting in the US: a Deep Learning study}, 
      author={Mehrdad Fazli and Heman Shakeri},
      year={2022},
      eprint={2212.08798},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```
