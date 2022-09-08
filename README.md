# data-segmentation



## Bug found :
With keras segmentation, I found a bug with the callback attribute in the train method. if you have the same bug, there are the steps you have to do (found thanks to [this  issue](https://github.com/matterport/Mask_RCNN/issues/1968)):
1. In your virtual environnement (or local, but I highly recommend using venv) to the file `[my_venv]\Lib\site-packages\keras_segmentation\train.py`
2. Add `callbacks=None,` in the parameter of the train function (maybe it's already here)
3. Add the following block of code in the train function, after the `if validate: ...` block
```python
    if callbacks is None and (not checkpoints_path is  None) :
        default_callback = ModelCheckpoint(
                filepath=checkpoints_path + ".{epoch:05d}",
                save_weights_only=True,
                verbose=True
            )

        if sys.version_info[0] < 3: # for pyhton 2 
            default_callback = CheckpointsCallback(checkpoints_path)

        callbacks = [
            default_callback
        ]

    if callbacks is None:
        callbacks = []
```