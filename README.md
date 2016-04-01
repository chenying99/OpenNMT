# Sequence-to-Sequence Learning with Attentional Neural Networks

Implementation of a standard sequence-to-sequence model with attention where the encoder-decoder
are LSTMs. Also has the option to use characters on the input side (output is still at the
word-level) by doing a CNN+Highway over character embeddings to use as inputs.

## Dependencies

You will need the following packages:
* hdf5
* nngraph

GPU usage will additionally require:
* cutorch
* cunn

If running the character model, you should also install:
* cudnn
* luautf8

## Quickstart

We are going to be working with some example data in `data/` folder.
First run the data-processing code

```
python preprocess.py --srcfile data/src-train.txt --targetfile data/targ-train.txt
--srcvalfile data/src-val.txt --targetvalfile data/targ-val.txt --outputfile data/demo
```

This will take the source/target train/valid files (`src-train.txt, targ-train.txt,
src-val.txt, targ-val.txt`) and make some hdf5 files to be consumed by Lua.

`demo.src.dict`: Dictionary of source vocab to index mappings
`demo.targ.dict`: Dictionary of target vocab to index mappings
`demo-train.hdf5`: hdf5 containing the train data
`demo-val.hdf5`: hdf5 file containing the validation data

Now run the model

```
th train.lua -data_file data/demo-train.hdf5 -val_data_file data/demo-val.hdf5 -savefile demo-model
```
You can also add `-gpuid X` to use the X-th GPU in the cluster.

Now you have a model which you can use to predict on new data. To do this we are
going to be running beam search

```
th beam.lua -srcfile demo/src-train.txt -outfile pred.txt -srcdict demo/demo.src.dict
-targdict demo.targ.dict
```
This will output predictions into `pred.txt`. The predictions are going to be quite terrible,
as the demo dataset is small. Try running on some larger datasets.

## The Details

