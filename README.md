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

* [original paper](https://www.researchgate.net/publication/387495516_A_Methodology_to_Rank_Importance_of_Frequencies_and_Channels_in_Electromyography_Data_with_Decision_Tree_Classifiers)
* [Github](https://github.com/Slauva/Ranking-frequencies-and-channels-of-EMG)