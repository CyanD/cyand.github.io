---
layout: post
title: Some Tricks about TensorFlow PyTorch Python and C++
---

Here I share some tricks about TensorFlow PyTorch Python and C++ to speedup coding and developing.

# TensorFlow 1.x

## 1. Print variables and shape of weight in the saved checkpoint

``` BASH
python -m tensorflow.python.tools.inspect_checkpoint --file_name <model ckpt>
```

# Sox

## Change wav sampling rate in batch

``` BASH
input_wav_scp=input.wav.scp
output_wav_scp=output.wav.scp

nthread=48
count=0

while read -u 3 -r input && read -u 4 -r output; do
    dir=$(dirname ${output})
    mkdir -p $dir
    sox ${input} -c 1 -r 16000 -b 16 ${output} &
    count=$[$count + 1]
    [ $count -eq $nthread ] && count=0 && wait
done 3<$input_wav_scp 4<$output_wav_scp
```

* Put the above content to a `.sh` file, then use `bash <file.sh>` to execute.

**Python implementation**

``` Python
import numpy as np
import sys, os
import librosa
from scipy.io.wavfile import write
from multiprocessing.pool import Pool
from tqdm import tqdm

input_wav_scp = sys.argv[1]
output_wav_scp = sys.argv[2]

def process(input_output):
    input, output = input_output
    os.makedirs(os.path.dirname(output), exist_ok=True)
    x = librosa.load(input, sr=16000)[0]
    x *= 32767 / max(0.01, np.max(np.abs(x)))
    write(output, 16000, x.astype(np.int16))

inputs = [line.strip() for line in open(input_wav_scp).readlines()]
outputs = [line.strip() for line in open(output_wav_scp).readlines()]
inputs_outputs = zip(inputs, outputs)
job = Pool(os.cpu_count()).imap(process, inputs_outputs)
list(tqdm(job, "Process", len(inputs), unit=" wav"))
print("Done!")
```

# Commonly used Linux command

## 1. String replacement in a text file

``` bash
sed 's/word1/word2/g' input.file > output.file
```

Use `-i` to replace in place.

## 2. Loop over two files

``` bash
while read -u 3 -r file1 && read -u 4 -r file2; do
  echo $file1 $file2
  …
done 3<list1.txt 4<list2.txt
```

## 3. Git set and unset http proxy

Set http proxy

``` bash
git config --global http.proxy http://proxyUsername:proxyPassword@proxy.server.com:port
```

Check current configuration

``` bash
git config --global --get-regexp http.*
```

Unset http proxy

``` bash
git config --global --unset http.proxy
```

## 4. Find all broken soft link

Find all broken soft links

``` bash
find ./ -xtype l
```

Find and then delete all broken soft links

``` bash
find ./ -xtype l -delete
```

OR

``` bash
find ./ -xtype l -exec rm {} \;
```

The `\;` flag is needed to identify the end of executing command.

Note `find` command can be used with more complex executing command, for example:

``` bash
find www/*.html -type f -exec sh -c "echo $(basename {})" \;
```

## 5. Split a large file into multiple small files

Use the `split` command.

Example 

``` bash
split -l 20 --additional-suffix=.txt --suffix-length=3 -d ../cleaned_user.txt speaker-
```

## 6. Convert multiple `.pcm` audios into `wav` format

Use find and sox command:

The `pcm` waves have `.raw` suffix names.

``` bash
find ./ -name '*.raw' -exec sh -c 'base=$(basename {} .raw); sox -r 16000 -e signed -b 16 -c 1 $base.raw $base.wav' \;
```

If the `pcm` waves have `.pcm` suffix names, then, add `-t raw` as the first argument of `sox` .

``` bash
find ./ -name '*.pcm' -exec sh -c 'base=$(basename {} .pcm); sox -t raw -r 16000 -e signed -b 16 -c 1 $base.pcm $base.wav' \;
```
