# 4. Optimize the model

Mobile devices have significant limitations, so any pre-processing that can be done to reduce an app's footprint is worth considering.

### Limited libraries on mobile

One way the TensorFlow library is kept small, for mobile, is by only supporting the subset of operations that are commonly used during inference. This is a reasonable approach, as training is rarely conducted on mobile platforms. Similarly it also excludes support for operations with large external dependencies. You can see the list of supported ops in the [tensorflow/contrib/makefile/tf_op_files.txt](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/makefile/tf_op_files.txt) file.

By default, most graphs contain training ops that the mobile version of TensorFlow doesn't support. . TensorFlow won't load a graph that contains an unsupported operation (even if the unsupported operation is irrelevant for inference).

### Optimize for inference

To avoid problems caused by unsupported training ops, the TensorFlow installation includes a tool, `optimize_for_inference`, that removes all nodes that aren't needed for a given set of input and outputs.

The script also does a few other optimizations that help speed up the model, such as merging explicit batch normalization operations into the convolutional weights to reduce the number of calculations. This can give a 30% speed up, depending on the input model. Here's how you run the script:

```
python -m tensorflow.python.tools.optimize_for_inference \
  --input=tf_files/retrained_graph.pb \
  --output=tf_files/optimized_graph.pb \
  --input_names="input" \
  --output_names="final_result"
```

Running this script creates a new file at `tf_files/optimized_graph.pb`.

### Verify the optimized model

To check that `optimize_for_inference` hasn't altered the output of the network, compare the `label_image` output for `retrained_graph.pb` with that of `optimized_graph.pb`:

```
python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb\
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

```
python -m scripts.label_image \
    --graph=tf_files/optimized_graph.pb \
    --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

When I run these commands I see no change in the output probabilities to 5 decimal places.

Now run it yourself to confirm that you see similar results.

### Investigate the changes with TensorBoard

If you followed along for the first tutorial, you should have a `tf_files/training_summaries/` directory (otherwise, just create the directory by issuing the following Linux command: `mkdir tf_files/training_summaries/`).

The following two commands will kill any runninng TensorBoard instances and launch a new instance, in the background watching that directory:

```
pkill -f tensorboard
tensorboard --logdir tf_files/training_summaries &
```

TensorBoard, running in the background, may occasionally print the following warning to your terminal, which you may safely ignore

`WARNING:tensorflow:path ../external/data/plugin/text/runs not found, sending 404\.`

Now add your two graphs as TensorBoard logs:

```
python -m scripts.graph_pb2tb tf_files/training_summaries/retrained \
  tf_files/retrained_graph.pb 

python -m scripts.graph_pb2tb tf_files/training_summaries/optimized \
  tf_files/optimized_graph.pb 
```

Now [open TensorBoard](http://0.0.0.0:6006/), and navigate to the "Graph" tab. Then from the pick-list labeled "Run"on the left side, select "Retrained".

Explore the graph a little, then select "Optimized" from the "Run" menu.

From here you can confirm some nodes have been merged to simplify the graph. You can expand the various blocks by double-clicking them.