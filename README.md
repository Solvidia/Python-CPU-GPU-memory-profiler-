<img width="661" alt="image" src="https://github.com/Solvidia/Python-CPU-GPU-memory-profiler-/assets/166895578/e8fe3ef5-a974-4e09-acd8-5138b8048950">


# solvidia-mem-prof: a Python CPU+GPU+memory profiler with AI-powered optimization proposals

by [Emery Berger](https://emeryberger.com), [Sam Stern](https://samstern.me/), and [Juan Altmayer Pizzorno](https://github.com/jaltmayerpizzorno).

 
## About solvidia-mem-prof

solvidia-mem-prof is a high-performance CPU, GPU *and* memory profiler for
Python that does a number of things that other Python profilers do not
and cannot do.  It runs orders of magnitude faster than many other
profilers while delivering far more detailed information. It is also
the first profiler ever to incorporate AI-powered proposed
optimizations.

### AI-powered optimization suggestions

> **Note**
>
> To enable AI-powered optimization suggestions, you need to enter an [OpenAI key](https://openai.com/api/) in the box under "Advanced options". _Your account will need to have a positive balance for this to work_ (check your balance at https://platform.openai.com/account/usage).
>
> <img width="487" alt="solvidia-mem-prof advanced options" src="https://user-images.githubusercontent.com/1612723/211639253-ec926b38-3efe-4a20-8514-e10dde94ec01.png">

Once you've entered your OpenAI key (see above), click on the lightning bolt (⚡) beside any line or the explosion (💥) for an entire region of code to generate a proposed optimization. Click on a proposed optimization to copy it to the clipboard.

<img width="571" alt="example proposed optimization" src="https://user-images.githubusercontent.com/1612723/211639968-37cf793f-3290-43d1-9282-79e579558388.png">

You can click as many times as you like on the lightning bolt or explosion, and it will generate different suggested optimizations. Your mileage may vary, but in some cases, the suggestions are quite impressive (e.g., order-of-magnitude improvements). 
  
### Quick Start

#### Installing solvidia-mem-prof:

```console
python3 -m pip install -U solvidia-mem-prof
```

or

```console
conda install -c conda-forge solvidia-mem-prof
```

#### Using solvidia-mem-prof:

After installing solvidia-mem-prof, you can use solvidia-mem-prof at the command line, or as a Visual Studio Code extension.

<details>
  <summary>
    Using the solvidia-mem-prof VS Code Extension:
  </summary>
  

First, install <a href="https://marketplace.visualstudio.com/items?itemName=EmeryBerger.solvidia-mem-prof">the solvidia-mem-prof extension from the VS Code Marketplace</a> or by searching for it within VS Code by typing Command-Shift-X (Mac) or Ctrl-Shift-X (Windows). Once that's installed, click Command-Shift-P or Ctrl-Shift-P to open the <a href="https://code.visualstudio.com/docs/getstarted/userinterface">Command Palette</a>. Then select <b>"solvidia-mem-prof: AI-powered profiling..."</b> (you can start typing solvidia-mem-prof and it will pop up if it's installed). Run that and, assuming your code runs for at least a second, a solvidia-mem-prof profile will appear in a webview.
  
<img width="734" alt="Screenshot 2023-09-20 at 7 09 06 PM" src="https://github.com/plasma-umass/solvidia-mem-prof/assets/1612723/7e78e3d2-e649-4f02-86fd-0da2a259a1a4">

</details>

<details>
<summary>
Commonly used command-line options:
</summary>

```console
solvidia-mem-prof your_prog.py                             # full profile (outputs to web interface)
python3 -m solvidia-mem-prof your_prog.py                  # equivalent alternative

solvidia-mem-prof --cli your_prog.py                       # use the command-line only (no web interface)

solvidia-mem-prof --cpu your_prog.py                       # only profile CPU
solvidia-mem-prof --cpu --gpu your_prog.py                 # only profile CPU and GPU
solvidia-mem-prof --cpu --gpu --memory your_prog.py        # profile everything (same as no options)

solvidia-mem-prof --reduced-profile your_prog.py           # only profile lines with significant usage
solvidia-mem-prof --profile-interval 5.0 your_prog.py      # output a new profile every five seconds

solvidia-mem-prof (solvidia-mem-prof options) --- your_prog.py (...) # use --- to tell solvidia-mem-prof to ignore options after that point
solvidia-mem-prof --help                                   # lists all options
```

</details>

<details>
<summary>
Using solvidia-mem-prof programmatically in your code:
</summary>

Invoke using `solvidia-mem-prof` as above and then:

```Python
from solvidia-mem-prof import solvidia-mem-prof_profiler

# Turn profiling on
solvidia-mem-prof_profiler.start()

# Turn profiling off
solvidia-mem-prof_profiler.stop()
```

</details>

<details>
<summary>
Using solvidia-mem-prof to profile only specific functions via <code>@profile</code>:
</summary>

Just preface any functions you want to profile with the `@profile` decorator and run it with solvidia-mem-prof:

```Python
# do not import profile!

@profile
def slow_function():
    import time
    time.sleep(3)
```

</details>

#### Web-based GUI

solvidia-mem-prof has both a CLI and a web-based GUI [(demo here)](http://plasma-umass.org/solvidia-mem-prof-gui/).

By default, once solvidia-mem-prof has profiled your program, it will open a
tab in a web browser with an interactive user interface (all processing is done
locally). Hover over bars to see breakdowns of CPU and memory
consumption, and click on underlined column headers to sort the
columns. The generated file `profile.html` is self-contained and can be saved for later use.

[![solvidia-mem-prof web GUI](https://raw.githubusercontent.com/plasma-umass/solvidia-mem-prof/master/docs/solvidia-mem-prof-gui-example.png)](https://raw.githubusercontent.com/plasma-umass/solvidia-mem-prof/master/docs/solvidia-mem-prof-gui-example-full.png)


## solvidia-mem-prof Overview

### solvidia-mem-prof talk (PyCon US 2021)

[This talk](https://youtu.be/5iEf-_7mM1k) presented at PyCon 2021 walks through solvidia-mem-prof's advantages and how to use it to debug the performance of an application (and provides some technical details on its internals). We highly recommend watching this video!

[![solvidia-mem-prof presentation at PyCon 2021](https://raw.githubusercontent.com/plasma-umass/solvidia-mem-prof/master/docs/images/solvidia-mem-prof-video-img.png)](https://youtu.be/5iEf-_7mM1k "solvidia-mem-prof presentation at PyCon 2021")

### Fast and Accurate

- solvidia-mem-prof is **_fast_**. It uses sampling instead of instrumentation or relying on Python's tracing facilities. Its overhead is typically no more than 10-20% (and often less).

- solvidia-mem-prof is **accurate**. We tested CPU profiler accuracy and found that solvidia-mem-prof is among the most accurate profilers, correctly measuring time taken.



- solvidia-mem-prof performs profiling **_at the line level_** _and_ **_per function_**, pointing to the functions and the specific lines of code responsible for the execution time in your program.

### CPU profiling

- solvidia-mem-prof **separates out time spent in Python from time in native code** (including libraries). Most Python programmers aren't going to optimize the performance of native code (which is usually either in the Python implementation or external libraries), so this helps developers focus their optimization efforts on the code they can actually improve.
- solvidia-mem-prof **highlights hotspots** (code accounting for significant percentages of CPU time or memory allocation) in red, making them even easier to spot.
- solvidia-mem-prof also separates out **system time**, making it easy to find I/O bottlenecks.

### GPU profiling

- solvidia-mem-prof reports **GPU time** (currently limited to NVIDIA-based systems).

### Memory profiling

- solvidia-mem-prof **profiles memory usage**. In addition to tracking CPU usage, solvidia-mem-prof also points to the specific lines of code responsible for memory growth. It accomplishes this via an included specialized memory allocator.
- solvidia-mem-prof separates out the percentage of **memory consumed by Python code vs. native code**.
- solvidia-mem-prof produces **_per-line_ memory profiles**.
- solvidia-mem-prof **identifies lines with likely memory leaks**.
- solvidia-mem-prof **profiles _copying volume_**, making it easy to spot inadvertent copying, especially due to crossing Python/library boundaries (e.g., accidentally converting `numpy` arrays into Python arrays, and vice versa).

### Other features

- solvidia-mem-prof can produce **reduced profiles** (via `--reduced-profile`) that only report lines that consume more than 1% of CPU or perform at least 100 allocations.
- solvidia-mem-prof supports `@profile` decorators to profile only specific functions.
- When solvidia-mem-prof is profiling a program launched in the background (via `&`), you can **suspend and resume profiling**.

# Comparison to Other Profilers

## Performance and Features

Below is a table comparing the **performance and features** of various profilers to solvidia-mem-prof.

![Performance and feature comparison](https://raw.githubusercontent.com/plasma-umass/solvidia-mem-prof/master/docs/images/profiler-comparison.png)

- **Slowdown**: the slowdown when running a benchmark from the Pyperformance suite. Green means less than 2x overhead. solvidia-mem-prof's overhead is just a 35% slowdown.

solvidia-mem-prof has all of the following features, many of which only solvidia-mem-prof supports:

- **Lines or functions**: does the profiler report information only for entire functions, or for every line -- solvidia-mem-prof does both.
- **Unmodified Code**: works on unmodified code.
- **Threads**: supports Python threads.
- **Multiprocessing**: supports use of the `multiprocessing` library -- _solvidia-mem-prof only_
- **Python vs. C time**: breaks out time spent in Python vs. native code (e.g., libraries) -- _solvidia-mem-prof only_
- **System time**: breaks out system time (e.g., sleeping or performing I/O) -- _solvidia-mem-prof only_
- **Profiles memory**: reports memory consumption per line / function
- **GPU**: reports time spent on an NVIDIA GPU (if present) -- _solvidia-mem-prof only_
- **Memory trends**: reports memory use over time per line / function -- _solvidia-mem-prof only_
- **Copy volume**: reports megabytes being copied per second -- _solvidia-mem-prof only_
- **Detects leaks**: automatically pinpoints lines responsible for likely memory leaks -- _solvidia-mem-prof only_

## Output

If you include the `--cli` option, solvidia-mem-prof prints annotated source code for the program being profiled
(as text, JSON (`--json`), or HTML (`--html`)) and any modules it
uses in the same directory or subdirectories (you can optionally have
it `--profile-all` and only include files with at least a
`--cpu-percent-threshold` of time).  Here is a snippet from
`pystone.py`.

![Example profile](https://raw.githubusercontent.com/plasma-umass/solvidia-mem-prof/master/docs/images/sample-profile-pystone.png)

* **Memory usage at the top**: Visualized by "sparklines", memory consumption over the runtime of the profiled code.
* **"Time Python"**: How much time was spent in Python code.
* **"native"**: How much time was spent in non-Python code (e.g., libraries written in C/C++).
* **"system"**: How much time was spent in the system (e.g., I/O).
* **"GPU"**: (not shown here) How much time spent on the GPU, if your system has an NVIDIA GPU installed.
* **"Memory Python"**: How much of the memory allocation happened on the Python side of the code, as opposed to in non-Python code (e.g., libraries written in C/C++).
* **"net"**: Positive net memory numbers indicate total memory allocation in megabytes; negative net memory numbers indicate memory reclamation.
* **"timeline / %"**: Visualized by "sparklines", memory consumption generated by this line over the program runtime, and the percentages of total memory activity this line represents.
* **"Copy (MB/s)"**: The amount of megabytes being copied per second (see "About solvidia-mem-prof").

##  solvidia-mem-prof

The following command runs solvidia-mem-prof on a provided example program.

```console
solvidia-mem-prof test/testme.py
```

<details>
 <summary>
  Click to see all solvidia-mem-prof's options (available by running with <code>--help</code>)
 </summary>

```console
    % solvidia-mem-prof --help
     usage: solvidia-mem-prof [-h] [--outfile OUTFILE] [--html] [--reduced-profile]
                    [--profile-interval PROFILE_INTERVAL] [--cpu-only]
                    [--profile-all] [--profile-only PROFILE_ONLY]
                    [--use-virtual-time]
                    [--cpu-percent-threshold CPU_PERCENT_THRESHOLD]
                    [--cpu-sampling-rate CPU_SAMPLING_RATE]
                    [--malloc-threshold MALLOC_THRESHOLD]
     
     solvidia-mem-prof: a high-precision CPU and memory profiler.
     https://github.com/plasma-umass/solvidia-mem-prof
     
     command-line:
        % solvidia-mem-prof [options] yourprogram.py
     or
        % python3 -m solvidia-mem-prof [options] yourprogram.py
     
     in Jupyter, line mode:
        %scrun [options] statement
     
     in Jupyter, cell mode:
        %%solvidia-mem-prof [options]
        code...
        code...
     
     optional arguments:
       -h, --help            show this help message and exit
       --outfile OUTFILE     file to hold profiler output (default: stdout)
       --html                output as HTML (default: text)
       --reduced-profile     generate a reduced profile, with non-zero lines only (default: False)
       --profile-interval PROFILE_INTERVAL
                             output profiles every so many seconds (default: inf)
       --cpu-only            only profile CPU time (default: profile CPU, memory, and copying)
       --profile-all         profile all executed code, not just the target program (default: only the target program)
       --profile-only PROFILE_ONLY
                             profile only code in filenames that contain the given strings, separated by commas (default: no restrictions)
       --use-virtual-time    measure only CPU time, not time spent in I/O or blocking (default: False)
       --cpu-percent-threshold CPU_PERCENT_THRESHOLD
                             only report profiles with at least this percent of CPU time (default: 1%)
       --cpu-sampling-rate CPU_SAMPLING_RATE
                             CPU sampling rate (default: every 0.01s)
       --malloc-threshold MALLOC_THRESHOLD
                             only report profiles with at least this many allocations (default: 100)
     
     When running solvidia-mem-prof in the background, you can suspend/resume profiling
     for the process ID that solvidia-mem-prof reports. For example:
     
        % python3 -m solvidia-mem-prof [options] yourprogram.py &
      solvidia-mem-prof now profiling process 12345
        to suspend profiling: python3 -m solvidia-mem-prof.profile --off --pid 12345
        to resume profiling:  python3 -m solvidia-mem-prof.profile --on  --pid 12345
```
</details>

### solvidia-mem-prof with Jupyter

<details>
<summary>
Instructions for installing and using solvidia-mem-prof with Jupyter notebooks
</summary>

[This notebook](https://nbviewer.jupyter.org/github/plasma-umass/solvidia-mem-prof/blob/master/docs/solvidia-mem-prof-demo.ipynb) illustrates the use of solvidia-mem-prof in Jupyter.

Installation:

```console
!pip install solvidia-mem-prof
%load_ext solvidia-mem-prof
```

Line mode:

```console
%scrun [options] statement
```

Cell mode:

```console
%%solvidia-mem-prof [options]
code...
code...
```
</details>

## Installation

<details open>
<summary>Using <code>pip</code> (Mac OS X, Linux, Windows, and WSL2)</summary>

solvidia-mem-prof is distributed as a `pip` package and works on Mac OS X, Linux (including Ubuntu in [Windows WSL2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-index)) and (with limitations) Windows platforms.

> **Note**
>
> The Windows version currently only supports CPU and GPU profiling, but not memory or copy profiling.
> 

You can install it as follows:
```console
  % pip install -U solvidia-mem-prof
```

or
```console
  % python3 -m pip install -U solvidia-mem-prof
```

You may need to install some packages first.

See https://stackoverflow.com/a/19344978/4954434 for full instructions for all Linux flavors.

For Ubuntu/Debian:

```console
  % sudo apt install git python3-all-dev
```
</details>

<details>
<summary>Using <code>conda</code> (Mac OS X, Linux, Windows, and WSL2)</summary>

```console
  % conda install -c conda-forge solvidia-mem-prof
```

solvidia-mem-prof is distributed as a `conda` package and works on Mac OS X, Linux (including Ubuntu in [Windows WSL2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-index)) and (with limitations) Windows platforms.

> **Note**
>
> The Windows version currently only supports CPU and GPU profiling, but not memory or copy profiling.
> 
</details>

<details>
<summary>On ArchLinux</summary>

You can install solvidia-mem-prof on Arch Linux via the [AUR
package](https://aur.archlinux.org/packages/python-solvidia-mem-prof-git/). Use your favorite AUR helper, or
manually download the `PKGBUILD` and run `makepkg -cirs` to build. Note that this will place
`libsolvidia-mem-prof.so` in `/usr/lib`; modify the below usage instructions accordingly.
</details>

# Frequently Asked Questions

<details>
<summary>
Can I use solvidia-mem-prof with PyTest?
</summary>

**A:** Yes! You can run it as follows (for example):

`python3 -m solvidia-mem-prof --- -m pytest your_test.py` 

</details>

<details>
<summary>
Is there any way to get shorter profiles or do more targeted profiling?
</summary>

**A:** Yes! There are several options:

1. Use `--reduced-profile` to include only lines and files with memory/CPU/GPU activity.
2. Use `--profile-only` to include only filenames containing specific strings (as in, `--profile-only foo,bar,baz`).
3. Decorate functions of interest with `@profile` to have solvidia-mem-prof report _only_ those functions.
4. Turn profiling on and off programmatically by importing solvidia-mem-prof (`import solvidia-mem-prof`) and then turning profiling on and off via `solvidia-mem-prof_profiler.start()` and `solvidia-mem-prof_profiler.stop()`. By default, solvidia-mem-prof runs with profiling on, so to delay profiling until desired, use the `--off` command-line option (`python3 -m solvidia-mem-prof --off yourprogram.py`).
</details>

<details>
<summary>
How do I run solvidia-mem-prof in PyCharm?
</summary>

**A:**  In PyCharm, you can run solvidia-mem-prof at the command line by opening the terminal at the bottom of the IDE and running a solvidia-mem-prof command (e.g., `python -m solvidia-mem-prof <your program>`). Use the options `--cli`, `--html`, and `--outfile <your output.html>` to generate an HTML file that you can then view in the IDE.
</details>

<details>
<summary>
How do I use solvidia-mem-prof with Django?
</summary>

**A:** Pass in the `--noreload` option (see https://github.com/plasma-umass/solvidia-mem-prof/issues/178).
</details>


<details>
<summary>
Does solvidia-mem-prof work with gevent/Greenlets?
</summary>

**A:** Yes! Put the following code in the beginning of your program, or modify the call to `monkey.patch_all` as below:

```python
from gevent import monkey
monkey.patch_all(thread=False)
```
</details>



<details>
<summary>
How do I use solvidia-mem-prof with PyTorch on the Mac?
</summary>

**A:** solvidia-mem-prof works with PyTorch version 1.5.1 on Mac OS X. There's a bug in newer versions of PyTorch (https://github.com/pytorch/pytorch/issues/57185) that interferes with solvidia-mem-prof (discussion here: https://github.com/plasma-umass/solvidia-mem-prof/issues/110), but only on Macs.
</details>

# Technical Information

For details about how solvidia-mem-prof works, please see the following paper, which won the Jay Lepreau Best Paper Award at [OSDI 2023](https://www.usenix.org/conference/osdi23/presentation/berger): [Triangulating Python Performance Issues with solvidia-mem-prof](https://arxiv.org/pdf/2212.07597). (Note that this paper does not include information about the AI-driven proposed optimizations.)

<details>
<summary>
To cite solvidia-mem-prof in an academic paper, please use the following:
</summary>

```latex
@inproceedings{288540,
author = {Emery D. Berger and Sam Stern and Juan Altmayer Pizzorno},
title = {Triangulating Python Performance Issues with {S}calene},
booktitle = {{17th USENIX Symposium on Operating Systems Design and Implementation (OSDI 23)}},
year = {2023},
isbn = {978-1-939133-34-2},
address = {Boston, MA},
pages = {51--64},
url = {https://www.usenix.org/conference/osdi23/presentation/berger},
publisher = {USENIX Association},
month = jul
}
```
</details>


# Success Stories

If you use solvidia-mem-prof to successfully debug a performance problem, please [add a comment to this issue](https://github.com/plasma-umass/solvidia-mem-prof/issues/58)!


# Acknowledgements

Logo created by [Sophia Berger](https://www.linkedin.com/in/sophia-berger/).

This material is based upon work supported by the National Science
Foundation under Grant No. 1955610. Any opinions, findings, and
conclusions or recommendations expressed in this material are those of
the author(s) and do not necessarily reflect the views of the National
Science Foundation.
