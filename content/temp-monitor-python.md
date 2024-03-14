+++
title = "Temperature monitor for Raspberry Pi written in Python"
date = 2020-05-19

[taxonomies]
tags = ["Python", "Raspberry Pi"]
+++

During heavy load, it's common that the Raspberry Pi throttles its CPU and GPU due to too high temperatures being reached. Therefore, it can be of interest to monitor the temperatures of these components. Due to the low specs of the Raspberry, the extra load of running code to monitor these temperatures can unfortunately cause a rise in the temperatures of the components. In this post, I briefly present an application written in Python by expanding on four main points, which are:

* How to read the temperatures of the CPU and GPU.
* How to re-read the temperatures at a given interval.
* How to stop the re-reading.
* What module to use in order to create the GUI.
<!-- more -->

# How to read the temperatures of the CPU and GPU

To check the GPU temperature, run the following command in the terminal:

```bash
vcgencmd measure_temp
```

which will return something like

```bash
temp=56.3’C
```

To check the CPU temperature, run the following command in the terminal:

```bash
cat /sys/class/thermal/thermal_zone0/temp
```

which will return something like

```bash
53215
```

which is the temperature in Celsius multiplied by 1000.

# How to re-read the temperatures at a given interval

In order to re-read the temperature at a given interval, the module `Timer` is used. `Timer` takes the arguments of what function to run and how many seconds it should wait before running the function That is, if the following line of code is run

```python
Timer(interval=5, function=check_temperatures)
```

the function `check_temperatures` will be run after 5 seconds. How do we go about if we want to perpetually run `check_temperatures` every 5 seconds, and not just once? By including the above line of code in the function `check_temperatures` itself, running `check_temperatures` will queue a new call to this function, which in turn will queue a new call.

If the function which is called takes arguments, e.g. `check_temperatures(x, y)`, these need to be supplied to `Timer` like

```python
Timer(interval=5, function=check_temperatures, args=x, y)
```

# How to stop the re-reading

The Timer-call, more specifically the whole script, can be killed with the following command

```python
os.kill(os.getpid(), signal.SIGTERM)
```

This is done in order to prevent queued Timer-calls from being executed. To catch errors from Timer-calls being run after the application is closed, of the type `RuntimeError`, and then killing the process in order to prevent further calls, the following piece of code can be used:

```python
except RuntimeError:
os.kill(os.getpid(), signal.SIGTERM)
```

# What module to use in order to create the GUI

Due to the simplicity of the GUI monitor and `Tkinter` being preinstalled with Raspbian OS, `Tkinter` was chosen instead of `PyQt`.

# Source

The code for the application can be found [here](https://github.com/vcrn/temperature-monitor-raspberry).
