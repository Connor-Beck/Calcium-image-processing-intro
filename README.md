# Calcium imaging and image processing introduction

*Developed by Connor L. Beck*\
Updated July 2025

This repository is to support introductions into calcium imaging.

*Only modules 0 and 1 are currently available - more will be incorporated over time*

# Module 0 -  Introduction to Calcium imaging

Calcium imaging is used in neuroscience as a proxy for neural activity, as calcium acts as a secondary signaling molecule for neural function.

There is significant flexiblity with a growing list of methods across the chemical space of calcium binding, optical space of calcium imaging, and software space of signal processing. While great for those of us who have been working in the space for years, this can create a steep learning curve to get up to speed. I am hoping that I can use this as a platform to help smooth that curve by developing some practice modules that explain the working principles of calcium imaging and image processing.


## 0.1 How does Calcium imaging work?
[Grienberger and Konnerth](papers/Grienberger_and_Konnerth-Calcium.pdf) provide a great review on the biological background into neuronal calcium signaling and the practical approaches for imaging. I recommend reading through this work to learn about calcium indicators (both chemical and genetically encoded), dye-loading techniques, and imaging modalities.

Calcium imaging requires:
1. Host tissue: Typically neurons in live animals or cultured cells.
2. Calcium-sensitive sensors: Genetically encoded indicators are introduced via viral (non-replicating) injection or transgenic lines. Chemical sensors (e.g., Fluo, Oregon Green, Calbryte) are also common but limited to short experiments (<6 hours) and typically not done *in vivo*.
3. Optical system: Microscopes and imaging setups to excite and detect fluorescence. Can be 1-photon or 2-photon excitation.
4. Signal processing: Analysis pipelines to extract and interpret fluorescence changes from videos.

## 0.2 Understanding calcium signals in neurons
[Ali and Kwan](papers/Ali_and_Kwan-CalciumToBehavior.pdf) wrote a useful review explaining the differences of in vivo calcium imaging signals from the neuronal soma, axons, and dendrites, and the physiological relevance of these differences. They also do a great job addressing how to relate calcium signals to behavior, and major pitfalls you can run into on the analysis side.

Here we will primarily focus on somatic calcium imaging:

GCaMP6s expressing, Layer 2/3 pyramidal neurons in the motor cortex as the mouse explores.
![](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbzRoa2F3NGw0aGVmN2V6NmZrb204ZnlpcXF3a29yMmVhN2czYmd1dCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/N6944MoGCGbldk6POi/giphy.gif)

**Somatic** (neuronal cell body) calcium imaging can provide insights into neuronal activity, as voltage-gated calcium channels enable rapid calcium influx into the soma when the neuron fires.

However, calcium imaging in dendrites is also critical for translating neural function.

**Dendritic** (receiving terminals of neurons) calcium imaging can provide insights into synaptic inputs, as local voltage changes elict local calcium influx that can be quantified relative to the magnitude and number of simultaneous synaptic inputs.

Side note: What is discussed in this repository is only a small subset of what can be done with calcium imaging. There are many other uses of calcium imaging in neurons and other cell types. In my PhD, I used calcium imaging to track [changes in neuronal function under mechanical forces](https://onlinelibrary.wiley.com/doi/full/10.1002/smll.202406678). Calcium can be used to track functional changes in [astrocytes](https://www.cell.com/cell-reports/fulltext/S2211-1247(22)01100-7?dgcid=raven_jbs_etoc_email), [osteocytes](https://www.pnas.org/doi/10.1073/pnas.1707863114), or even [plants](https://www.cell.com/trends/plant-science/fulltext/S1360-1385(23)00233-9) . It's a brillant technique that has changed the scientific landscape!

## 0.3 Why do we care about calcium and neural dynamics?
The cerebral cortex is essential for mammalian behavior. The cortex can process sensory information, control voluntary movements, and execute high-order cognitive function, along with many other functions. These functions are governed by global and local neural circuits that modulate neural function to produce behavioral outputs. 

As systems neuroscientists, our goal is to identify the circuits that establish behaviors. Dissecting these circuits requires (1) tracking of neural activity in the circuit relative to the behavior and (2) modulation of neural activity during the behavior. We won't address modulating activity in this work, but recording circuits gives very little circuit information without the ability to probe components of it.

By tracking activity during behavior, we can identify key circuit features that are representative of behavior. Circuit features can be a number of features like local field potentials (beta, theta delta, etc.), local neurotransmitter concentrations (dopamine, seratonin, orexin, etc.), or individual neuronal firing (excitatory or inhibitory). Across these featues, there is a growing toolbox of recording modalities. Electrodes can be used to record neuronal electrophysiology with techniques like electroencephalogram (EEG), electrocorticography (ECoG), or shank recordings. The critical downside of electrophysiology is its lack of spatial resolution, which limits the recordings of indivdual neuronal activitys within large populations of neurons. To alleviate this, optical methods like calcium imaging or voltage imaging provide clear access to individual neurons. 

To overcome these limitations, optical methods such as calcium imaging and voltage imaging have emerged as powerful tools. These techniques enable direct visualization of activity from identified neurons across large fields of view, providing the spatial resolution needed to dissect cellular-level circuit dynamics in behaving animals. With calcium imaging, we can monitor the activity of hundreds to thousands of  neurons with cellular resolution, tracking their dynamics across time and linking these patterns to specific behavioral events. This approach allows us to move toward understanding the coordinated, population-level dynamics that give rise to perception, decision-making, and action.

In the cortex, coordinated firing of neurons is often referred to as an **ensemble**—a group of neurons that fire together to represent a specific percept, action, or cognitive state. Calcium imaging enables us to visualize these ensembles directly, revealing patterns of simultaneous activity among neurons that can be linked to specific behavioral events or internal computations. By identifying and characterizing these ensembles, we move beyond the study of single-neuron responses to uncover the collective dynamics that underlie perception, decision-making, and action. For a thorough discussion of why systems neuroscience must shift from studying individual neurons to investigating neural circuits as integrated networks, see [Yuste’s review](papers/Yuste-NeurontoNetwork.pdf), which provides historical context and a compelling argument for this approach.


# Module 1 - Exploring somatic calcium dynamics
Learning the entire calcium processing pipeline can be overwhelming to start with.

To start, I will provide you with raw calcium traces extracted from a calcium video like the one in 0.2.

**In this section, I would like you to explore calcium signals by designing your own processing scripts**\
*Calcium signal processing is typically done in either MATLAB or python. The field is very slowly moving towards python, but MATLAB is still commonly used. For this work, the choice is yours.*

Below are the classical methods to calcium image processing that you can follow. There is a level of subjectiveness to this data processing that depends on the calcium sensor, optics, and general noise of your recordings. Calcium signals and events always need to be validated in our research, which is why you will often see traces in publication figures.

If you would like to read about the general methods for calcium signal processing, [Saito et al.](papers/Saito_etal-NeuralCode2P.pdf) provides an in depth explanation into the neural codes that can be extracted from calcium imaging data. I often come back to this.

## Task 1.0 - Importing data
Here, I will provide you with calcium traces that have been extracted from a video. In the future, we will go through extraction, where I mostly recommend you use well established pipelines like [Suite2P](https://github.com/MouseLand/suite2p).

The [Fluorescent Data](data/Fluorescent%20Data.csv) csv file contains a number of calcium traces that you can explore.\
The dimensions of the data within the csv are **324 neurons x 15000 frames**.\
Each value in the data is the raw mean intensity recorded from neurons expressing GCAMP6s recorded at 4.8 frames per second.

## Task 1.1 - Signal cleaning
**Goal:**\
Calcium signals are often noisy and challenging to work with. Here, we will follow the standard preprocessing steps to clean up our signals. 

### Step 1.1.0 Normalize the signal
Compute	$ΔF/F$ as a percentage change from baseline intensity:

$$ΔF[t]/F = \frac{F[t] - F_0}{F_0}$$  
    
where $F_0$ is the resting calcium intensity when no firing occurs.

*hint:* $F_0$ *can be challenging to identify without knowing when events occur. Sometimes a lower quartile can be helpful.*
    
### Step 1.1.1 Subtract background noise
Slow (>20s) fluctuations in intensity can happen due to the optical instrumentation. It is common practice to remove these background fluctuations from the calcium signal.

*hint: It can be done with filtering, or lower envelope extractions, but should not remove the short term transient spikes from the signals.*

### Step 1.1.2 Denoise signal
Calcium signals (especially in high-frequency 20+ Hz recordings) will present with noise variance in the signal. These signals are cleaner as the sampling rate is lower.

*hint: wavelet denoising is a common approach.*

**Deliverable**\
Plot a couple of neuronal calcium signals across the steps (before, after normalization, after background subtraction, and after denoising) to ensure it works correctly.

## Task 1.2 - Detect transient calcium events
**Goal:**\
Now with clean signals, we will search for calcium events, markers of significant neuronal activity.

It is important to note that a **calcium event**  $\neq$  **neuron spike**

Calcium events are strongly correlated to spiking activity, but are not exact. Serveral [algorithms](https://www.jneurosci.org/content/38/37/7976) have been developed to deconvolve neuronal spiking from calcium traces, which we will explore in the future.

Here, we will prioritize tracking calcium events. 

Identify the time points when rapid calcium influx occurs. This will be represented by a strong and short-term intensity increase in the calcium signal. 
A single GCaMP6s calcium event should present a FWHM of < 5s, though repeated spikes can compound transient events, stacking intensities in time, but you will still see individal peaks.

*hint: for detection, we often use a neuron specific standard deviation threshold detection (e.g. an event is detected when the calcium signal is above 5 standard deviations from the mean). Sometimes, it can be necessary to add a second discrete threshold to correct for neurons that are minimally active over the recording window.*

*If you want to challenge yourself, there are other methods like template matching or wavelet ridgewalking that work robustly.*

**Deliverables**\
(1) Plot some neuronal calcium signals with highlighted locations of the transient events.\
(2) Create a scatter plot with time as the x-axis, neuron ID as the y-axis, with scatter points representing the time points corresponding to detected spikes.\
(3) Extract calcium transient features. Average spike rate across the neurons, mean firing rate over the recording, etc. Get creative! Try and come up with a metric I haven't heard of.

## Task 1.3 - Extracting network features.
**Goal:**\
Now that you've learned how to clean calcium signals and detect events, we'll move on to detecting **neuronal ensembles**.

A neuronal ensemble is a group of neurons that show coordinated activity within a brief time window. Rather than analyzing single neurons in isolation, we want to identify when and which neurons participate together in a potentially meaningful network event. While an ensemble is defined as the contributing neurons, an **ensemble event** is the particular time frame for which the ensemble fires. In our recorded data, we do not have the prior knowledge of what an ensemble is, so we start by identifying these ensemble events, and then use them to build a map of the ensembles. If you want to read more about this, [Wenzel and Hamm](papers/Wenzel_and_Hamm-Ensembles.pdf) provides a guide to identifying and quantifying neuronal ensembles with calcium imaging, discussing theoretical, experimental, and analytical considerations for studying co-activity patterns in vivo.

### Step 1.3.0 Compute neuronal coactivity.
To locate ensemble events, we will start by looking at the coactivity of all the neurons over the recording window. 

Neuronal events are not exactly synchronous, so, we bin events within short temporal windows (e.g., 500 ms). Then within this bin window, track the number of neurons presenting events as a coactivity marker. 

**Deliverable**\
Plot coactivity across time bins to visualize periods of high network activity.

### Step 1.3.1 Identify Ensemble Events
To determine which coactive events are significant, we generate a null distribution from our datasets.

If you take the transient events and circularly shift each neuron’s event train (random selection of the cut point), you will preserve possible autocorrelations in the dataset but destroy phyiological coactivity. Then, if you compute the neuronal coactivity on this shuffled dataset, you will create a null distribution of coactive events.  

Repeatedly generate these shuffled datasets (e.g., 1000 shuffles) to identify an indepth probability distribution of coactive magnitudes within the null distribution. 

Now, you can compare the real coactivity magnitudes to the null distribution, and determine significant coactive events as events surpassing the 99th percentile of the null distribution.

At this point, we assign these significant coactivity events as ensemble events.\
The neurons active in each ensemble event are *contributing neurons* to that ensemble event.

### Step 1.3.2 Identify ensembles

Not all ensemble events represent unique ensembles. For behavioral relevance, ensembles should fire repeatedly under specific behaviors, so we want to identify which ensembles are similar. Unfortunately (for our analysis), the brain introduces large variability, so ensembles do not always maintain exactly the same neurons, creating considerable challenges for matching ensemble events. 

The primary conceptual flow for identifying similar ensemble events is to compute a similarity metric of the neurons contributing to each ensemble event and then cluster them based on their similarity (or dissimilarity) to determine which ensemble events are similar enough in neuronal contributions to be described as an ensemble. 

**Determine contributing neuron similarity between ensemble events**\
Compute pairwise Jaccard similarity between binary vectors of neuron contributions across ensemble frames.

**Deliverable**\
Visualize the similarity ($S$) in matrix form:

$$S[x,y] = \text{jaccard similarity}(\text{ensemble event }_x>,\text{ensemble event }_y)$$
    
as a heatmap to see similarity structure.

**Cluster similar ensemble events**\
Then, with similarites across ensemble events, you can group similar ensemble events using some for of clustering (e.g., hierarchical clustering).\
It can be challenging to determine the cut-off for what gets clustered together. One method would be to determine the number of dimensonal components that explain a significant portion of the variance in PCA space.

Once you select a cutoff, ensemble events are clustered together, providing you with ensembles, each representing a recurring motif of coordinated neuronal activity.


**Deliverable**\
Present the ensembles and ensemble events.

Which neurons contribute to the ensemble?
When is this ensemble active in the dataset?




## Contributing

Pull requests are welcome. Please open an issue if you would like to help contribute to this project or if you find errors.

## License

[MIT](https://choosealicense.com/licenses/mit/)
