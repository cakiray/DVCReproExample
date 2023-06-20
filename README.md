# DVCReproExample
DVC Repro Example Codebase

## Commands for this tutorial
```
> git clone https://code.dvc.org/get-started/code.git
> cd code 
> dvc get https://github.com/iterative/dataset-registry get-started/data.xml -o data/data.xml
> dvc stage add -n prepare \
                -p prepare.seed,prepare.split \
                -d src/prepare.py -d data/data.xml \
                -o data/prepared \
                python src/prepare.py data/data.xml
> dvc stage add -n featurize \
                -p featurize.max_features,featurize.ngrams \
                -d src/featurization.py -d data/prepared \
                -o data/features \
                python src/featurization.py data/prepared data/features
> dvc stage add -n train \
                -p train.seed,train.n_est,train.min_split \
                -d src/train.py -d data/features \
                -o model.pkl \
                python src/train.py data/features model.pkl
> dvc stage add -n evaluate \
  -d src/evaluate.py -d model.pkl -d data/features \
  -M eval/live/metrics.json -O eval/live/plots \
  -O eval/prc -o eval/importance.png \
  python src/evaluate.py model.pkl data/features
  
> dvc repro
> dvc metrics show
> dvc plots show
> ##### Change features.max_features and featurize.ngrams 
> dvc repro
> dvc params diff
> dvc metrics diff
> dvc plots diff 
```

### Note for proxy users

Instead of dvc get ... , follow these commands
```
> mkdir data
> git clone https://github.com/iterative/dataset-registry
> cd dataset-registry/get-started
> dvc pull data.xml.dvc
> mv data.xml ../../data
```
