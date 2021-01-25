This is an implementation of Sea-Thru. The original paper by Derya Akkaynak and Tali Treibitz can be found [here](http://csms.haifa.ac.il/profiles/tTreibitz/webfiles/sea-thru_cvpr2019.pdf). Data can be found [here](http://csms.haifa.ac.il/profiles/tTreibitz/datasets/sea_thru/index.html).

You will need python3 as well as all the packages in the `requirements.txt` file, as well as tkinter (on Ubuntu, `sudo apt install python3-tk`).

I extended their method to use a CNN-based monocular depth estimation technique called monodepth2, which can be found [here](https://github.com/nianticlabs/monodepth2).
Check out the report for more information!

Example:

```
# install the requirements
pip3 install -r requirements.txt
# download the dataset
wget "https://www.dropbox.com/sh/xtimf7qwfak4wwc/AAAGqn2JMe98II9lYeBTVBE2a/D3?dl=1"
unzip D3.zip
# generate the image
# can use either RAW files or PNG/JPG
python3 seathru.py --image Raw/T_S04858.ARW --depth-map depthMaps/depthT_S04858.tif

## If you want to run the monocular depth estimation pipeline
# install deps
git submodule update --init --recursive
python3 seathru-mono-e2e.py --monodepth-add-depth 1.0 --monodepth-multiply-depth 3.0 --image Raw/T_S04858.ARW --raw --output output-monodepth.png
```

Input:

![](images/input.png?raw=true)


Output:

![](images/output.png?raw=true)


Output with estimated depths (ranging from 1.0-4.0 meters):

![](images/output-monodepth.png?raw=true)


The monodepth pipeline can also be used with PNG/JPEG images by omitting the `--raw` option. For each image, the `--monodepth-add-depth` parameter controls the minimum depth
that the estimation will output, and the `--monodepth-multiply-depth` controls the range of depths as follows:

```
output_depths = monodepth_multiply_depth + (monodepth_multiply_depth * monodepth_depths)
```

where `monodepth_depths` range from 0 to 1.

This option also works well as a dehazing routine for foggy scenes:

Original:

<img src="images/fog.jpg" width=320>

Output:

<img src="images/fog-output.png" width=320>
