# Calcium imaging and image processing introduction

Developed by Connor L. Beck\
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

**Somatic** (neuronal cell body) calcium imaging can provide insights into neuronal activity, as voltage-gated calcium channels enable rapid calcium influx into the soma when the neuron fires.

**Dendritic** (receiving terminals of neurons) calcium imaging can provide insights into synaptic inputs, as local voltage changes elict local calcium influx that can be quantified relative to the magnitude and number of simultaneous synaptic inputs.

Side note: What is discussed in this repository is only a small subset of what can be done with calcium imaging. There are many other uses of calcium imaging in neurons and other cell types. In my PhD, I used calcium imaging to track [changes in neuronal function under mechanical forces](https://onlinelibrary.wiley.com/doi/full/10.1002/smll.202406678). Calcium can be used to track functional changes in [astrocytes](https://www.cell.com/cell-reports/fulltext/S2211-1247(22)01100-7?dgcid=raven_jbs_etoc_email), [osteocytes](https://www.pnas.org/doi/10.1073/pnas.1707863114), or even [plants](https://www.cell.com/trends/plant-science/fulltext/S1360-1385(23)00233-9) (to name a few). It's a brillant technique that has changed the scientific landscape!

# Module 1 - Exploring calcium signal traces


## 1.0 Translating calcium signals to track neural codes
[Saito et al.](papers/Saito_etal-NeuralCode2P.pdf) is an in depth explanation into the neural codes that can be extracted from calcium imaging data. I often come back to this.


## 1.1 - Processing calcium signals
Learning the entire calcium processing pipeline can be a lot to start with.\
Instead, let's start with raw fluorescent calcium traces.

This data processing is typically done in either MATLAB or python, the choice is yours. The field is very slowly moving towards python, but MATLAB is still commonly used.

The [Fluorescent Data](data/Fluorescent%20Data.csv) csv file contains a number of calcium traces that you can explore.\
The dimensions of the data within the csv are **324 neurons x 15000 frames**.\
Each value in the data is the raw mean intensity recorded from neurons expressing GCAMP6s recorded at 4.8 frames per second.

### Task 1.1.1 - Signal cleaning
**Normalize the signal**\
Compute	$ΔF/F$ as a percentage change from baseline intensity:

$$ΔF[t]/F = \frac{F[t] - F_0}{F_0}$$  
    
where $$F_0$$ is the resting calcium intensity when no firing occurs.
    
**Subtract background noise**\
Slow (>20s) fluctuations in intensity can happen due to the optical instrumentation. It is common practice to remove these background fluctuations from the calcium signal.

*hint: It can be done with filtering, or lower envelope extractions, but should not remove the short term transient spikes from the signals.*

**Deliverable**\
Plot a couple of neuronal calcium signals across the steps (before, after normalization, and after background subtraction) to ensure it works correctly.

### Task 1.1.2 - Detect transient calcium events
Identify the time points when rapid calcium influx occurs. This will be represented by a strong and short-term intensity increase in the calcium signal. A single GCaMP6s calcium spike should present a FWHM of < 5s, though repeated spikes can compound transient events, stacking intensities in time, but you will still see individal peaks.

*hint: for detection we often use a neuron specific standard deviation threshold detection (e.g. an event is detected when the calcium signal is above 5 standard deviations from the mean).*

**Deliverables**\
(1) Plot a coule of neuronal calcium signals with highlighted locations of the transient events
(2) Create a scatter plot with time as the x-axis, neuron ID as the y-axis, with scatter points representing the time points corresponding to detected spikes.

### Task 1.2 - Analyze network activity

[Yuste's](papers/Yuste-NeurontoNetwork.pdf) review discussing the need for circuit investigations instead of individual neurons. A great history lesson and a fun read.

[Wenzel and Hamm](papers/Wenzel_and_Hamm-Ensembles.pdf) provides a guide to identifying and quantifying neuronal ensembles with calcium imaging, discussing theoretical, experimental, and analytical considerations for studying co-activity patterns in vivo.


## Contributing

Pull requests are welcome. Please open an issue if you would like to help contribute to this project or find errors.

## License

[MIT](https://choosealicense.com/licenses/mit/)
![image](https://github.com/user-attachments/assets/9d884419-fe78-4dfa-b9ed-a09382fb35e2)
