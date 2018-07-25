# install py-faster-rcnn with python3

### install caffe

Check the [guide](https://github.com/dungba88/caffe-python3-install/blob/master/install-caffe.md) for complete instruction. 

But there are some changes needed to run py-faster-rcnn. You will need to merge commit [0dcd397b29507b8314e252e850518c5695efbb83](https://github.com/rbgirshick/caffe-fast-rcnn/commit/0dcd397b29507b8314e252e850518c5695efbb83) after checking out the caffe code.

```shell
git remote add caffe-fast-rcnn git://github.com/rbgirshick/caffe-fast-rcnn.git
git fetch caffe-fast-rcnn
git cherry-pick -n 0dcd397b29507b8314e252e850518c5695efbb83
```

Another change required to fix `AttributeError: can't set attribute` in caffe.Net is patched here https://github.com/andpol5/caffe/commit/1bbcb0605fe67525a8493fb477d30119893a0978

### install py-faster-rcnn (with GPU support)

Checkout from https://github.com/dungba88/py-faster-rcnn. It includes fixes for Python3 compatibility.

There is no special change required to install with GPU support. Just follow the guide and you'll be fine.

### install py-faster-rcnn (without GPU support)

Checkout from https://github.com/dungba88/py-faster-rcnn. It includes fixes for Python3 compatibility.

Make the following changes for CPU only
- in lib/setup.py, uncomment `CUDA = locate_cuda()` and the `nms.gpu_nms` extension
- in lib/fast-rcnn/config.py set `USE_GPU_NMS` to False

Then follow the rest of the [original guide](https://github.com/dungba88/py-faster-rcnn/blob/master/README.md)!
