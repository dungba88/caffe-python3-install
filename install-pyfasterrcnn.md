# install py-faster-rcnn with python3

### install caffe

Check the [guide](https://github.com/dungba88/caffe-python3-install/blob/master/install-caffe.md) for complete instruction. 

But there are some changes needed to run py-faster-rcnn. You will need to merge commit [0dcd397b29507b8314e252e850518c5695efbb83](https://github.com/rbgirshick/caffe-fast-rcnn/commit/0dcd397b29507b8314e252e850518c5695efbb83) after checking out the caffe code.

```shell
git remote add caffe-fast-rcnn git://github.com/rbgirshick/caffe-fast-rcnn.git
git fetch caffe-fast-rcnn
git cherry-pick -n 0dcd397b29507b8314e252e850518c5695efbb83
```
