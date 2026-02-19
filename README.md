# MuscleRecovery
This project investigates muscle recovery using surface electromyography (EMG) recorded from the vastus lateralis muscle during squat exercises. The goal is to classify different rest intervals (1, 5, and 10 minutes) and identify the most informative EMG frequencies and channels.

The dataset contains raw EMG time-domain signals along with event annotations marking squat and stand periods.

## Experimental Design
* Each participant completed three experimental blocks corresponding to different rest intervals:
    * 1-minute rest
    * 5-minute rest
    * 10-minute rest
* Each block structure: `Sample 1 → rest K minutes → Sample 2 → rest K minutes → Sample 3`
* Between blocks, participants rested for 10 minutes.
* Each sample consisted of:
    * Baseline recording
    * Three sequences:
        * Squat (3 seconds)
        * Stand (3 seconds)


## About Dataset
This dataset contains electromyography (EMG) signals recorded from the vastus lateralis muscle during squat exercises. The signals were collected over varying rest intervals to assess optimal recovery periods. The primary goal of this dataset is to help in the development of machine learning models that can classify rest intervals.

```sh
data/ — directory containing original data
data/1/ — each directory stands for a specific patient id
data/1/1min — "1min/5min/10min" each repository stands for a specific resting interval
data/1/1min/exp_1_points.json — file with timestamps for squat faces
data/1/1min/exp_1.csv — file with EMG data
participants.csv — study group description
```

### participants.csv

| ID | Gender | Age | Handiness |
|----|--------|-----|-----------|
| 1 | Male | 22 | right |
| 2 | Female | 21 | right |
| 3 | Male | 21 | right |
| 4 | Male | 22 | right |
| 5 | Male | 21 | right |

### exp_*.csv

| time | emg1 | emg2 | emg3 |
|------|------|------|------|
| 0.033366 | 2.976044 | 1.189451 | 0.400513 |
| 0.049346 | 1.521465 | 0.469817 | 0.190989 |
| 0.056792 | 1.521465 | 0.469817 | 0.190989 |
| 0.061424 | 1.521465 | 0.469817 | 0.190989 |
| 0.061567 | 1.521465 | 0.469817 | 0.190989 |

* Sampling rate: 1000 Hz
* Each row represents one sample (1 ms resolution)
* emg1–emg3 correspond to three electrodes placed on vastus lateralis

### exp_*_points.json

| File | baseline | close_0 | open_0 | close_1 | open_1 | close_2 | open_2 |
|------|----------|---------|--------|---------|--------|---------|--------|
| data/1/10min/exp_1_points.json | 5.194982 | 5.194987 | 8.215003 | 11.127866 | 14.157181 | 17.079057 | 20.108287 |
| data/1/10min/exp_2_points.json | 5.205411 | 5.205415 | 8.236081 | 11.158928 | 14.188139 | 17.100311 | 20.129508 |
| data/1/10min/exp_3_points.json | 5.203765 | 5.203770 | 8.234359 | 11.157408 | 14.175607 | 17.087606 | 20.106401 |
| data/1/1min/exp_1_points.json | 5.191146 | 5.191150 | 8.220996 | 11.144419 | 14.173022 | 17.085436 | 20.114560 |
| data/1/1min/exp_2_points.json | 5.205699 | 5.205703 | 8.235017 | 11.157987 | 14.187257 | 17.109618 | 20.139184 |

* `close_*` = squat start
* `open_*` = stand start


## Methods

### Signal Processing Pipeline
1. Baseline correction
2. Bandpass filter (20–500 Hz)
3. Notch filter at 50 Hz
4. Segment signal according to event markers
5. Compute Power Spectral Density (PSD)
6. Extract frequency-domain features (1–450 Hz for each channel)
7. Build classification dataset
8. Train Decision Tree classifier
9. Apply Rank Frequency analysis to identify important channel-frequency pairs

### Rank Frequency Analysis
After training multiple decision trees:
1. Select top-performing 10% classifiers
2. Evaluate feature stability across trees
3. Compute ranking score combining:
    * Cross-validation performance
    * Feature importance
    * Tree rank
    * Feature rank within tree
This identifies the most robust and informative frequency-channel combinations.

## To Do
* visualize data
* reproduce outcomes in paper
* try to improve performance
    * try other classifier (spike-based PSTH is not applicable, but event-aligned time bin features can be explored.)
    * Apply PCA for dimensionality reduction

## Reference
* [original paper](https://www.researchgate.net/publication/387495516_A_Methodology_to_Rank_Importance_of_Frequencies_and_Channels_in_Electromyography_Data_with_Decision_Tree_Classifiers)
* [Github](https://github.com/Slauva/Ranking-frequencies-and-channels-of-EMG)