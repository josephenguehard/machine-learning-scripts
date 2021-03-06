# Using Jupyter notebooks on Puhti

This page contains instructions on setting up a Jupyter notebooks server with GPU support in Puhti. It can be used e.g. to run GPU-enabled TensorFlow, Keras, PyTorch, or Rapids notebooks.

Please note that this is a rather inelegant solution based on using two ssh connections, but can still perhaps be useful in some cases.

In this example, we use 8899 as the port number, but please select a unique port to avoid 
overlaps.  You are free to select a suitable port from the range 1024-49151. 

## First terminal window (runs Jupyter notebook server):

    ssh -l USERNAME puhti.csc.fi

Set up a suitable module environment, for example:

    module purge
    module load tensorflow

More information about data analytics and machine learning environments available on Puhti can be found at https://docs.csc.fi/#apps/#data-analytics-and-machine-learning .

The following `srun` command reserves CPUs and GPUs and opens a shell on one of the compute nodes.  The
option `-t` sets the time limit in the format `HH:MM:SS`, the option `--mem` sets the memory 
reservation, `--gres=gpu:v100:X` reserves `X` GPUs (1<=`X`<=4), and `-c Y` reserves `Y` CPU cores. Replace also `project_xxx` with your compute project.

    srun -A project_xxx -p gpu --gres=gpu:v100:1 -c 10 -t 02:00:00 --mem=64G --pty $SHELL
    hostname  # you need this information later
    jupyter-lab --no-browser --port=8899

## Second terminal window (for SSH port forwarding):

    ssh -l USERNAME -L 8899:localhost:8899 puhti.csc.fi
    ssh -L 8899:localhost:8899 rXXgYY  # use output of “hostname” command above

## Browser:

Point your browser to the URL given in the first terminal window, e.g.:

http://localhost:8899/?token=c828f3351d0b76ccde12759b942d3ed3c622955e94d6cdc8
