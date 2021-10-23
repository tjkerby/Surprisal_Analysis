# Surprisal_Analysis

## Background
Surprisal Analysis has been used for a while in engineering to try to decompose systems that want to be in a state of maximal entropy given constraints. A classic example would be modeling gas inside of a chamber with a piston. The gas naturally wants to be evenly distributed accross all the volume. If the piston (ie. constraint) moves and reduces or increases the volume in the chamber the gas naturally wants to fill it up. Thus it is always in a state of maximal entropy.

One of the first methods to apply this to biological data was (Remacle et al., [2010](https://www.pnas.org/content/pnas/107/22/10324.full.pdf)). They argue that molecules within a cell often behave similarly to the gas example. A little bit after this paper other scientists began using this method on gene expression data sets. A good recent example of a paper published doing this is (Su et al., [2020](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7214418/)). However, it wasn't until about 2020 that this method was applied to raman spectrometry data (Du et al., [2020](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7518429/)). 

A collaborator that I have been working with, Dr. Zhou at USU, was interested if this method could be used to discriminate cells that had been subjected to different treatments as well as give inference into the nature of the treatment. After reading through the previous literature as well a very helpful review article (Remacle et al., [2016](https://www.mdpi.com/1099-4300/18/12/445/htm)), I determined that this method could be a beneficial one for him. 

## Main Concept
Surprisal Analysis is actually a very simple method. They transform their data by taking the natural log of each element in a dataset and essentially apply SVD. There are a lot of reasons that explain why this works well and (Remacle et al., [2010](https://www.pnas.org/content/pnas/107/22/10324.full.pdf)) does a good job of explaining them if you're interested in more details. The gist of it is that thermodynamic entropy does a better job of estimating the baseline entropy of your system when there are no constraints applied to it. To limit your entropy distribution to this one, the natural log transformation is applied. Once you have estimated the baseline entropy well all that is left to explain are the different constraints (treatments) applied to your system.

I was surprised after reading all of these papers that I could not find a working version of surprisal analysis in python or r. So I implemented one myself. It could definitely be improved and scaled, but I feel that what I have is sufficient for my needs for the time being.

This file, Raman_Germondetal_data.ipynb shows me applying my version of surprisal analysis to a public dataset. I didn't use Dr. Zhou's data simply because he has not published it yet. However, though this data is not quite as interesting, it still gives you the main idea. However, I can show you some of the figures from my analysis thus far. 

## Results
If you were to plot the average raman waves by each treatment you would see the graph below. 

![DEP Levels for Raman Data](https://github.com/tjkerby/Surprisal_Analysis/blob/main/Figures/Comparing_Wavelengths_of_DEP_Levels.jpeg)

Once surprisal analysis is applied to the data, we essentially have a different representation for our data where instead of the over 1000 raman wave measurements we have new components and a scalor value for each component. Using just 5 components we a pretty good reconstruction for each group as seen in the figure below.

![Groups Original vs Reconstructed](https://github.com/tjkerby/Surprisal_Analysis/blob/main/Figures/Groups_original_vs_reconstructed.jpeg)

When we plot a heatmap of the lambda values vs observations within a treatment group (see figure below) we can see that the first lambda covers the baseline raman wave since all groups have high values for it. However, the 2nd lambda seem to have a trend where higher levels of DEP result in lower values of lambda 2. The 3rd lamdba also seems to have a slight trend where higher DEP levels result in higher values of lambda 3. 

![Heatmap of Lambdas vs Obs by Group](https://github.com/tjkerby/Surprisal_Analysis/blob/main/Figures/Heatmap_of_Lambda_by_Obersation_for_each_DEP_Level.jpeg)

You can see this trend better by plotting violin plots of the lambda values by treatment groups.

![Violin plot of lambdas by group](https://github.com/tjkerby/Surprisal_Analysis/blob/main/Figures/Violin_Plot_Distribution_of_Lambda_by_DEP_Level.jpeg)

Finally, we can view the effects of each lambda by looking at its values back in the original variable space. Below I've plotted lambdas 2 and 3 since these ones were associated with the treatment effect.

![Treatment effects of lambdas 2 and 3](https://github.com/tjkerby/Surprisal_Analysis/blob/main/Figures/Effects_of_Lambda_2_and_3.jpeg)

More work will be done on identifying the biological interpretations of these wavelengths. I also intend to edit my code to make this more scalable for those who wish to use it for their own analyses.
