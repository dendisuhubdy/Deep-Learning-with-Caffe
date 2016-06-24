#How to properly set up Imagenet Dataset for training in Caffe !

Assuming that you have the training and testing compressed files downloaded, let's move ahead and untar them properly.
  Make folders for training, testing and validation images. Make these where you want to store the dataset. If you are making these directories in a seperate partition, make sure that it has enough space to accomodate the files.

```
mkdir path/to/new/train/images/folder
mkdir path/to/new/test/images/folder
mkdir path/to/new/val/images/folder
```

Once you have the directories ready, we will start extracting the files.
Please note that extracting the `ILSVRC2012_img_test.tar` and `ILSVRC2012_img_val.tar` will give you images with `.JPEG` extension. However, extracting `ILSVRC2012_img_train.tar` and `ILSVRC2012_img_train_t3.tar` files will give you many more `.tar` files which you can extract according to your needs.

To extract the tar files, simply do these steps:

```
tar -xf ILSVRC2012_img_test.tar -C path/to/new/test/images/folder
tar -xf ILSVRC2012_img_val.tar -C path/to/new/val/images/folder
tar -xf ILSVRC2012_img_train.tar -C path/to/new/train/images/folder
tar -xf ILSVRC2012_img_train_t3.tar -C path/to/new/train_t3/images/folder
```
If you want to see what files are being extracted, use the `-v` flag along with `-xf`. So that would be `tar -xvf file.tar -C <destination folder>`.

Once you are done with the extraction, let us now discuss about the training images. Each `.tar` files in the training images folder corresponds to different set of images. You can train your network with any number of images from any number of these subsets. Here, however, I will explain how to extract every `tar` files in the train folder so that we can work with the entire dataset.

To extract all the `.tar` files in train folder, make a script file inside the train folder. The following script will extract all the `.tar` files into respective directories and will remove the `.tar` files after extraction completes. If you do not want to remove the compressed files, please comment the appropriate line of code.

Make sure you are in the train folder.
```
cd path/to/new/train/images/folder
```

Copy the following and paste it into a file (ex. `tar_extract_script.sh`).

```
#!/bin/bash
for f in *.tar; do
  d=`basename $f .tar`
  mkdir $d
  (cd $d && tar xf ../$f)
done
rm -r *.tar
```

Give it executable permissions. 
```
chmod 700 tar_extract_script.sh
```

Run the script. 
```
./tar_extract_script.sh
```

It will take a long time to extract all the files. If you want to see which files are being extracted, give the -v flag in the script.

Lets do some deep learning now!
