# 6. Optional Step: Trying Other Hyperparameters

The retraining script has several other command line options you can use.

You can read about these options in the help for the retrain script:

`python -m scripts.retrain -h`

Try adjusting some of these options to see if you can increase the final validation accuracy.

For example, the `--learning_rate` parameter controls the magnitude of the updates to the final layer during training. So far we have left it out, so the program has used the default learning_rate value of 0.01\. If you specify a small learning_rate, like 0.005, the training will take longer, but the overall precision might increase. Higher values of learning_rate, like 1.0, could train faster, but typically reduces precision, or even makes training unstable.

You need to experiment carefully to see what works for your case.

>It is very helpful for this type of work if you give each experiment a unique name, so they show up as separate entries in TensorBoard.
>
>It's the `--summaries_dir` option that controls the name in tensorboard. Earlier we used:
>
>`--summaries_dir=training_summaries/basic`
>
>TensorBoard is monitoring the contents of the `training_summaries` directory, so setting `--summarys_dir` to `training_summaries` or any subdirectory of `training_summaries` will work.
>
>You may want to set the following two options together, so your results are clearly labeled:
>
>`--learning_rate=0.5`
>`--summaries_dir=training_summaries/LR_0.5`