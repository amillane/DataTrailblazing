---
layout: post
title: "Support Vector Regression"
date: 2025-01-25
author: Drew Millane 
introduction: Support Vector Regression (SVR) is a powerful supervised learning technique that balances complexity and generalization by minimizing a regularized risk function. This document explores the theoretical foundations of SVR, its implementation, and its application in predictive modeling. A simulation study demonstrates the model's performance, followed by a practical application to real-world data. Key findings highlight the robustness and flexibility of SVR in handling nonlinear relationships and noisy datasets.
read_time: true
image:
  path: 
  thumbnail: 
  caption: 
---

# Introduction
Multivariate statistics is a branch of statistics that extends modeling to multidimensional spaces, where relationships between variables become increasingly complex and challenging to interpret. To address these complexities, various techniques and models are employed, such as MANOVA, discriminant analysis, and clustering. One particularly effective method, widely used in machine learning for accurate prediction, is support vector regression (SVR). This paper introduces SVR and evaluates its performance through a comparative analysis with other models in both a simulation study and a real-world application. 


# Background 

## Support Vector Machines
SVR is an extension of the classification method known as Support Vector Machines (SVM). A foundational understanding of SVM is essential to grasp the principles of SVR.

SVMs are a powerful classification method that works by finding a hyperplane that best separates data into their respective classes. To determine this boundary, SVMs create margins, parallel lines on either side of the hyperplane, representing the largest possible distance between the hyperplane and the nearest data points from each class. These critical data points, known as support vectors, define the position of the hyperplane. A classification decision rule assigns an observation to a group based on its position relative to the hyperplane.

<!-- Add Graph -->

What makes SVMs particularly powerful is their ability to handle real-world scenarios where classes are not perfectly separable. This is achieved by introducing slack variables, which allow some observations to fall on the wrong side of the margin or hyperplane. This flexibility prevents the hyperplane from being overly influenced by small subsets of data that could cause dramatic shifts in its position. In essence, SVMs sacrifice perfect separation of the data for a more robust and generalizable model as shown in Figure 1 \cite{James2013}.

To obtain a hyperplane that maximizes the margins but allows some observations to be misclassified the solution is optimized from this problem: 


<!-- Add Equations -->

A key aspect of this optimization problem is the C parameter, which controls the trade-off between maximizing the margin and allowing for misclassification or margin violations \cite{James2013}. Specifically, C acts as a constraint on the sum of the slack variables, determining how many violations to the margin are allowed and their severity. When C is small (approaching zero), the model prioritizes finding the widest possible margin with minimal tolerance for violations. This increases the risk of overfitting, as the model becomes too rigid. When C is large, the model tolerates more violations, resulting in wider margins and a greater risk of underfitting the data. To achieve the best performance, hyperparameter tuning is required to identify the optimal value of C.

Another notable property of this optimization problem is that only observations on or inside the margin—or those violating it—affect the position of the hyperplane \cite{James2013}. Observations that are correctly classified and lie comfortably outside the margin have no influence on the solution. This characteristic enhances the model's ability to be generalized, as it focuses on the most informative data points without being overly affected by the entire dataset.


# Method 

<!-- Add Graph -->

Similar to SVMs, SVR fits a hyperplane to the data. However, unlike SVMs, which are used for classification tasks, SVR is designed to handle continuous response variables. Instead of using margins, SVR incorporates an $\epsilon$-sensitive region which is designed to indicate the tolerance for error around the model as shown in Figure \ref{fig:SVR}. The goal of SVR is to find a function that best approximates the data within the sensitivity region while remaining as flat and simple as possible. However, this may not be plausible given that the region that is desired may still have observations outside of it or we want to allow for some error. 

Similar to SVMs, SVR introduces slack variables ($\xi$) for observations outside the $\epsilon$-sensitive region. The distances of these slack variables from the region are measured and incorporated into the optimization process. Observations within the $\epsilon$-region are ignored, while those outside influence the hyperplane's position, making them the support vectors \cite{EfficientLearningMachines2015}.

The optimization problem for a linear function is as follows:

<!-- Add Equations -->


The goal of this optimization problem is to minimize the weights $\mathbf{w}$, ensuring that the 
function is as flat (generalized) as possible \cite{ZHANG2020123}. Similar to the C parameter in SVMs, C in SVR controls the trade-off between the flatness of the function and the tolerance for deviations larger than $\epsilon$ \cite{Smola2004}. As C increases, the model places greater emphasis on minimizing prediction errors, whereas decreasing C prioritizes a simpler, flatter function. 

Additionally, the size of the region affects this optimization problem as well. As the size of this region increases, the model becomes less sensitive to prediction errors, resulting in a simpler approximated function. Conversely, a smaller region allows the model to capture more details, leading to a more complex approximation. The balance between C and $\epsilon$ is vital to find a model the best approximates the data, but also is robust with noise. 

The ability of SVR to generalize well and handle outliers effectively is one of its key advantages. It also performs well with high-dimensional data, making it a widely used tool for multivariate pattern recognition \cite{ZHANG2020123}. Additionally, SVR leverages kernels to identify the optimal hyperplane in cases of non-linear relationships in p-dimensional space. However, a notable drawback is that as the number of observations grows, its computational efficiency and scalability can become a limitation.

# Simulation 

A simulation study was conducted to evaluate Support Vector Regression (SVR) in handling outliers and modeling linear and nonlinear relationships, and then compared to Ordinary Least Squares (OLS) and random forests (RF).

Linear datasets featured simple linear effects, while nonlinear datasets included polynomial or sinusoidal effects. Leave-one-out cross-validation (LOOCV) was used to assess model performance, with SVR (linear, polynomial, and radial kernels), OLS, and RF tuned for optimal results.

The simulation varied the proportion of outliers (0, 0.05, 0.10, 0.20) and sample sizes (100, 500, 1000), generating 200 datasets per combination for both scenarios - a total of 2,400 datasets. This allowed a robust comparison of model performance across conditions.

## Results 

For the linear simulation shown in Figure \ref{fig:Linear}, the SVR linear kernel model demonstrates performance similar to OLS, with slightly better results as the proportion of outliers increases to 5\% and the sample size grows. This may be due to the tuning parameters that cause the model to behave similarly to OLS by minimizing the error across all observations. However, SVR linear does not significantly outperform OLS in scenarios with outliers, which was surprising. In certain cases, the SVR radial kernel performed the best, particularly as the sample size increased. An interesting pattern observed was that as the proportion of outliers increased, the performance differences between models diminished.

For the non-linear simulation shown in Figure \ref{fig:NonLinear}, the SVR radial kernel consistently outperformed the other models, with random forests (RF) ranking second in performance. The SVR polynomial kernel showed the poorest performance overall, suggesting that it may be more suited to simpler nonlinear relationships. As expected, the linear models, including OLS and SVR with a linear kernel, began to diverge from the nonlinear models as the sample size increased and the proportion of outliers grew, highlighting their limitations in capturing complex relationships and handling outliers. In particular, the radial kernel demonstrated a superior ability to handle outliers compared to RF, maintaining lower RMSE values under all conditions. This suggests that the radial kernel is particularly effective in scenarios involving nonlinear relationships and moderate levels of noise or outliers.

# Application

In a controlled environment, Support Vector Regression (SVR) demonstrated a decent prediction performance overall. To evaluate its applicability in a real-world scenario, a dataset was selected to compare the performance of SVR against random forest (RF) and ordinary least squares (OLS) regression.

The dataset, sourced from the UC Irvine Machine Learning Repository, consists of 9,568 data points collected over six years (2006-2011) from a combined cycle power plant. The features include hourly averages of temperature (T), ambient pressure (AP), relative humidity (RH), and exhaust fume (V), used to predict the net hourly electrical energy output (PE). Since these features are measured on different scales, the data was scaled prior to implementing any models to ensure comparability and optimize model performance.

As shown in Figure \ref{fig:EDA}, the AV plots show linear relationships between the characteristics and the target variable with some outliers. This provides an opportunity for the SVR to demonstrate its capabilities through its ability to generalize well. 

## Results

To evaluate the performance of the model, a 10-fold cross-validation was applied to the data set and the mean RMSE for each model was recorded. As shown in Table \ref{tab:AppResults}, the random forest model demonstrated the best predictive performance, with an RMSE of 3.17. The second-best model was the support vector regression (SVR) with a radial kernel. These results suggest that both models effectively captured some non-linear relationships within the features. However, the superior performance of the random forest may be attributed to the relatively low presence of outliers in the dataset, a scenario where SVR models typically excel. Another interesting result is that both the LM and the SVRL performed similarly, suggesting that neither outperforms the other in this context. Additional models can be implemented to assess how SVR compares with other approaches.

# Conclusion

In conclusion, Support Vector Regression (SVR) is designed as a machine learning model to address regression problems by enhancing generalization through the use of an epsilon-insensitive tube, which focuses on observations outside this region. Simulations demonstrate that the radial kernel performs particularly well for nonlinear relationships, while the linear kernel shows comparable performance to OLS but fails to outperform it as the sample size increases. This suggests that the advantages of SVR decrease with larger sample sizes. SVR appears to be most effective in scenarios with smaller data sets and the presence of outliers, where its robustness and ability to focus on critical data points shine.

The next steps would be to determine what type of scenarios the polynomial kernel performs well compared to the other kernels, as well as dive deeper into the impact of the tuning parameter C and $\epsilon$.


