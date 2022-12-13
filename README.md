Leveraging Wastewater Monitoring for COVID-19 Forecasting in the US: a Deep Learning study
==========================================================================================

Study Summary
-------------
COVID-19 forecasting has been proven a challenging task. Many researchers have turned to wastewater viral load as a pooled sample and an early indicator of an outbreak.
We propose the use of deep learning to automatically learn the relationship between daily confirmed cases and viral load data. We trained one Deep Temporal Convolutional Networks (DeepTCN) and one Temporal Fusion Transformer (TFT) model to build a global forecasting model. We supplemented the daily confirmed cases data with viral load data and other socio-economic factors as covariates and compared the models' performances. Our results suggest that TFT outperforms DeepTCN and learns a better association between viral load and daily cases. We showed that viral load could improve the MAE in TFT by more than ~2 units and in DeepTCN by 4 units. Similar improvements were also observed in other metrics. Moreover, viral load is shown to be the second most crucial input, following the containment and health index. Our results reveal the feasibility of training a location-agnostic deep learning model that can capture the dynamics of infection diffusion given appropriate covariates including, wastewater viral load data.

Proposed Models:
---------------

DeepTCN
-------


