---
layout: post
title: A simple guide to use Montreal Forced Aligner For Chinese Corpus
---

The Montreal Forced Alinger (MFA) [1] tool is based on Kaldi to perform forced alignment task.

This post shows my trial to do Chinese forced alignment using this tool. Most of the content is based on the official document [2].

# Pipeline

Four stages of training: 1) monophone model, 2) triphone model, 3) LDA+MLLT, which learns a transform of the features that makes each phone's features maximally different, 4) enhance the triphone model by taking into account the speaker difference, and calculate the transformation of MFCC features for each speaker.

# Data preparation

One should prepare 3 type of files, 1) pronunciation dictionary, 2) sound files, 3) orthographic annotation (in prosodylab-aligner or TextGrid intervals format). The sound files and the orthographic annotation should be contained in one directory structured as follows:

``` text
+-- corpus_directory
|       --- recording1.wav
|       --- recording1.TextGrid
|       --- recording2.wav
|       --- recording2.TextGrid
|       --- ...
```

Link [3] is a helpful script collection to preprocess various corpora of other format.

The default temporary directory is `~/Documents/MFA` , which would store all the logs, audio features, and other temporary files. By using the `-t` flag when calling the executable, you can change the default temporary directory.

## Pronunciation dictionary

The pronunciation dictionary is in the following format:

``` text
WORDA PHONEA PHONEB
WORDB PHONEB PHONEC
```

The separator between word and phoneme is white space.

Two special phones can be used for annotations that are not speech, `sil ` and `spn` . The `sil` phone is used to model silence (or similar to silence like breathing and exhalation), and `spn` for unknown words (like laughter or coughing).

``` text
{LG} spn
{SL} sil
```

## Sound files

The only supported format for sound files is `.wav` . The basename of the sound files and the annotation files should be the same, otherwise, they would be ignored. Sampling rate >=16k, and duration < 30s.

# Data validation

The data validation tool is useful to check your prepared corpus.

The data validation utility parses the corpus and the dictionary, prints out summary information about the corpus, and logs any of the following issues:

* OOV
* Wav file reading problem
* Transcription file reading problem
* Missing `.wav` files
* Missing `.lab/.TextGrid` files
* Sampling rate error
* Acoustic model error, skipped if `--ignore_acoustics` is flagged

Running the validation utility use this command

``` 
bin/mfa_validate_dataset corpus_directory dictionary_path [optional_acoustic_model_path]
```

I use soft links to link the source wav to the corpus directory. But sometimes, some soft links are broken. We should delete the broken soft links and the corresponding `.lab` file, here is the command to find and delete:

``` bash
find ./ -xtype l -exec sh -c 'rm {} $(basename {} .wav).lab' \;
```

Then, check if there exists broken soft links again:

``` bash
find ./ -xtype l
```

# Final model

The final model is in the `.zip` format and contains the following four files:

``` bash
final.mdl
final.occs
meta.yaml
tree
```

# Hands-on Examples

## Install MFA

``` bash
wget https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner/releases/download/v1.0.1/montreal-forced-aligner_linux.tar.gz
tar zxvf montreal-forced-aligner_linux.tar.gz
ln -s lib/libpython3.6m.so.1.0 lib/libpython3.6m.so
./bin/mfa_train_and_align # test
./bin/mfa_align # test
```

## Example 1: Chinese dictionary generate and train align model

The g2p model is given.

``` bash
wget http://mlmlab.org/mfa/CH_g2p_example.zip # mandarin corpus
wget http://mlmlab.org/mfa/mfa-models/g2p/mandarin_pinyin_g2p.zip # g2p model
unzip CH_g2p_example.zip.zip # g2p model is in zip format
# generate dictionary from g2p model
../bin/mfa_generate_dictionary  mandarin_pinyin_g2p.zip examples/CH/ examples/chinese_dict.v2.txt
# align
../bin/mfa_train_and_align examples/CH/ examples/chinese_dict.v2.txt examples/aligned_output # issue 1
```

It's better to specify the `-o <output model.zip>` flag to store the trained model when calling `mfa_train_and_align` for future use.

If you want to change the default configurations of alignment or training, you can specify the `--config_path PATH` flag to a yaml config file.

## Example 2: Train Chinese G2P model

After example 1, one can start this example.

``` bash
../bin/mfa_train_g2p examples/chinese_dict.txt CH_test_model.zip # issue 2
```

## Example 3: Align using pre-trained model

After example 1, one can start this example.

``` bash
../bin/mfa_align examples/CH/ examples/chinese_dict.v2.txt ~/model.zip examples/aligned_output.v2
```

# Common Issues

## 1. FileNotFoundError: [Errno 2] No such file or directory: '/root/Documents/MFA/CH/train/mfcc/raw_mfcc.0.scp'

Referred the github issue: https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner/issues/91

To check the error log in `/root/Documents/MFA/CH/train/mfcc/log/make_mfcc.0.log` :

``` text
compute-mfcc-feats: error while loading shared libraries: libgfortran.so.3: cannot open shared object file: No such file or directory
copy-feats: error while loading shared libraries: libgfortran.so.3: cannot open shared object file: No such file or directory
```

The reason is that you should install `libgfortran3` .

**Solution**: `apt-get install libgfortran3`

## 2. FileNotFoundError: [Errno 2] No such file or directory: ''

This is because my file system is based on FDS. The output model cannot be in FDS. One fast solution is to change the output model to another place, hard-disk, SSD, etc.

## 3. aligner.exceptions. TrainerError: No field found for key fmllr_power

I have no idea how to fix this. But the first action I take is to do data validation. 

``` bash
./bin/mfa_validate_dataset <corpus> <lexicon> --ignore_acoustics -j 48
```

Here is the data validation result, but it seems like having no problems.

``` text
Setting up corpus information...
Creating dictionary information...
Setting up corpus_data directory...
Skipping acoustic feature generation

    =========================================Corpus=========================================
    0 sound files

    - sound files .lab transcription files

    0 sound files with TextGrids transcription files
    0 additional sound files ignored (see below)

    - speakers
    - utterances

    0.000 seconds total duration
    
    DICTIONARY
    ----------
    There were no missing words from the dictionary. If you plan on using the a model trained on this dataset to align other datasets in the future, it is recommended that there be at least some missing words.
    
    SOUND FILE READ ERRORS
    ----------------------
    There were no sound files that could not be read.
    
    FEATURE CALCULATION
    -------------------
    Acoustic feature generation was skipped.
    
    FILES WITHOUT TRANSCRIPTIONS
    ----------------------------
    There were no sound files missing transcriptions.
    
    TRANSCRIPTIONS WITHOUT FILES
    --------------------
    There were no transcription files missing sound files.
      
    TEXTGRID READ ERRORS
    --------------------
    There were no issues reading TextGrids.
    
    UNREADABLE TEXT FILES
    --------------------
    There were no issues reading text files.
    
    UNSUPPORTED SAMPLE RATES
    --------------------
    There were no sound files with unsupported sample rates.
    
Skipping test alignments.
```

Then, I checked the error info again, which told us that there's no `fmllr_power` field. I searched this keyword on the `MontrealCorpusTools` repo on GitHub, which showed that there's only one result.

![alt](/assets/image/github.scrennprint/fmllr_power.png)

Why is there only one search result? Since I specified `--config_path` to a custom config file, I think `fmllr_power` is an illegal option in the `yaml` configuration file, which should be removed. Let's try to run `mfa_train_and_align` command again without specifying a custom configuration.

# Reference

1. https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner
2. https://montreal-forced-aligner.readthedocs.io/
3. https://github.com/MontrealCorpusTools/MFA-reorganization-scripts
