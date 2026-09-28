### 27/9

**1. Run Théo's code only on MorphoMNIST**

MorphoMNIST is small and downloads easily. PadChest is over 100k images, so just begin with moprho i think.

- Follow the README, get the environment installed
- try to compute one or two metrics end to end.

**2. Download Harvard-GF and explore it**

- Start the download early, since it's 20.5 GB.
    - Downloaded it in the data/raw/dataset folder of the data_diversity_evaluation local folder that is a copy of theos repo. But also in my own repo under /data/dataset
    - Maybe move all code and data to my own repo that veronika can see, instead of just foler
- Load one `.npz` file in Python and look at what's inside: the RNFLT map, the B-scan shape, and the metadata fields.
    - We show the folder layout and if the dataset is really split or not.

    ```python
    import numpy as np, pandas as pd, glob
    from pathlib import Path

    data_dir = Path("../data/dataset")
    print(sorted(p.name for p in data_dir.iterdir())[:20])

    npz_files = sorted(glob.glob(str(data_dir / "**/*.npz"), recursive=True))
    print(len(npz_files), "npz files")

    d = np.load(npz_files[0], allow_pickle=True)
    for k in d.files:
        print(k, d[k].shape, d[k].dtype)

    #Result
    '.DS_Store', 'Test', 'Training', 'Validation']
    3300 npz files
    oct_bscans (200, 200, 200) uint8
    rnflt (200, 200) float64
    md () float64
    glaucoma () int64
    tds (52,) float64
    race () <U18
    male () int64
    hispanic () int64
    language () int64
    maritalstatus () int64
    age () float64
    ```

    We see the dataset is 3.300 npz files and it is split in training, validation and test data.  (As the dataset card also mentioned). However there are some differences form the dataset card fx the race being a 18-character Unicode string data type and the field “ethnicity” being called “hispanic” instead.

    - Now we build the metadata table (np.load() only loads the fileds we ask for so 3d images arent loaded)

    ```python
    from pathlib import Path

    rows = []
    for f in npz_files:
        z = np.load(f, allow_pickle=True)
        row = {k: z[k].item() for k in ["age", "male", "race", "hispanic", "language", "maritalstatus", "glaucoma", "md"]}
        row["split"] = Path(f).parent.name      # Training / Validation / Test
        row["file"] = Path(f).name
        rows.append(row)

    df = pd.DataFrame(rows)
    print(df["split"].value_counts())
    print(df["race"].value_counts())
    df.head()

    #Results
    split
    Training      2100
    Test           900
    Validation     300
    Name: count, dtype: int64
    race
    White or Caucasian           1100
    Black or African American    1100
    Asian                        1100
    Name: count, dtype: int64

    	age	    male	race	      hispanic,language,maritalstatus,glaucoma,md	split	file
    0	60.284932	1	White or Caucasian	0	0	0	0	-0.16	                      Test	data_2401.npz
    1	50.115068	0	White or Caucasian	1	0	1	0	0.59	Test	data_2402.npz
    2	27.890411	0	White or Caucasian	-1	0	1	0	0.37	Test	data_2403.npz
    3	47.621918	1	White or Caucasian	0	0	0	1	-3.68	Test	data_2404.npz
    4	52.528767	1	White or Caucasian	0	0	0	0	0.05	Test	data_2405.npz

    ```

    We note that missing values exists as they fx under hispanic would appear as -1 which we see row nr 2 have.

    - Now we look at summaries for the metadata columns and look at what numebers stand out. The summary we look at is the mean of glaucomas under each metadata as well as the total count.

    ```python
    cat_cols = ["race", "male", "hispanic", "language", "maritalstatus"]

    for col in cat_cols:
        print(f" {col}")
        print(df.groupby(col)["glaucoma"].agg(["mean", "count"]))
        print()

    #Result
      race
    mean  count
    race
    Asian                      0.481818   1100
    Black or African American  0.620909   1100
    White or Caucasian         0.486364   1100

    male
    mean  count
    male
    0     0.515453   1812
    1     0.547043   1488

    hispanic
    mean  count
    hispanic
    -1        0.484043    188
    0        0.530909   3025
    1        0.586207     87

    language
    mean  count
    language
    -1        0.548387     31
    0        0.500521   2877
    1        0.763158     38
    2        0.740113    354

    maritalstatus
    mean  count
    maritalstatus
    -1             0.543210     81
    0             0.515924   1884
    1             0.501584    947
    2             0.577381    168
    3             0.740331    181
    4             0.666667     39

    n_missing  pct_missing
    hispanic             188          5.7
    language              31          0.9
    maritalstatus         81          2.5
    age              0
    male             0
    race             0
    hispanic         0
    language         0
    maritalstatus    0
    glaucoma         0
    md               0
    split            0
    file             0
    dtype: int64

    ```

    We note that black or African American under race stands out with far more occurances of glaucoma. (Shortcut potential)

    “Widowed” and “other language” groups also have high rates, possibly confounded with age or race, to check. Widowed patients are likely older, and glaucoma rises with age, so marital status may just be standing in for age. Check if true, for example with `df.groupby("maritalstatus")["age"].mean()`.

    We also note the number of missing data in hispanic, language and maritial status data. These also feed the data diversity metric and we do know that Theo also used -1 for missingdata in padchest data.

    Now to have closer look at these things:

    ```python
    #Chack if age and martial status is connected in a way
    print(df.groupby("maritalstatus")["age"].mean())

    #Race by sex table
    print(df.groupby(["race", "male"])["glaucoma"].agg(["mean", "count"]))

    #Glaucoma rate by split and race, to see if the race gap holds in Training,
    # Validation and Test.
    print(df.groupby(["split", "race"])["glaucoma"].agg(["mean", "count"]))

    #Result
    maritalstatus
    -1    59.749180
     0    61.452132
     1    50.062919
     2    63.836679
     3    77.184744
     4    61.843123
    Name: age, dtype: float64
                                        mean  count
    race                      male
    Asian                     0     0.473950    595
                              1     0.491089    505
    Black or African American 0     0.620690    609
                              1     0.621181    491
    White or Caucasian        0     0.450658    608
                              1     0.530488    492
                                              mean  count
    split      race
    Test       Asian                      0.480000    300
               Black or African American  0.633333    300
               White or Caucasian         0.516667    300
    Training   Asian                      0.471429    700
               Black or African American  0.605714    700
               White or Caucasian         0.470000    700
    Validation Asian                      0.560000    100
               Black or African American  0.690000    100
               White or Caucasian         0.510000    100
    ```


Note:

Something to raise tomorrow: If you train on a single race, you have about 1,100 images in total, and roughly 700 of them in the training split. The paper's scanner subgroups had around 43,000 images each. Results on this little data will be noisier, so Veronika may have a view on how to handle it (repeated runs, or wider confidence intervals).

Still need to analyse more fx on md (Mean deviation value of visual field)
