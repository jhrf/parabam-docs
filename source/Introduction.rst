Preface - Quickstart
====================

For those who don't want to read the docs before jumping in, try the code snippets below to get going with parabam as quickly as possible.

First open your terminal and install parabam:

.. code-block:: shell

   sudo pip install parabam

Then in your terminal create a directory to run your parabam analysis on:

.. code-block:: shell

   cd ~
   mkdir parabam-analysis
   cd parabam-analysis

Now let's create an instruction file for our analysis. Create the file:

.. code-block:: shell

   touch instruction.py

Open the `instruction.py` file in your favourite text editor and paste this code into your new file:

.. code-block:: python

	def rule(read,constants,master):
		if "ATCGATCG" in read.seq:
			return True
		return False

Now locate the BAM file you wish to run parabam analysis on. Then type the following into your terminal command line:

.. code-block:: shell

	parabam subset -v2 -i instructions -b /path/to/bam/file_name.bam

After a while (depending on how large your input BAM file was) parabam will be finished and you will be left with a result in `<file_name>_subset.bam`.

Congratulations - you just ran parabam subset. Read on to find out more.

Introduction
============

So, you've got this far through the documentation which means you are at least reasonably curious about parabam. Great! 

The goal of parabam is to allow the user to inspect large BAM files in a timely manner whilst making the most of their computational resources and without having to write too much code. The most direct way to use parabam is via the command line, but it can also be invoked programaticaly via interface classes, and also support full incorporation via OOP inheritance.

If any of the words in the pragraph above don't make sense: *don't worry*. Parabam is easy to run for beginniners. All you need is to know a little bit of Unix (how to make and change directories) and a little bit of Python. Just follow the instructions and you'll be conducting high-throughput analysis in no time. If you have any questions, feel free to email me.

Alternatively, if you understood every word of the above and want to get more in-depth with parabam then pay close attention to the programmatic and inheritance based interfaces.

Installing parabam
++++++++++++++++++

First, we have to get parabam running on your computer. This section will guide you through installing parabam.

Currently, parabam is only available to POSIX compliant systems (Linux, Mac OSX, etc) and not Windows.

The easy way
------------

The easiest way to install parabam is as follows:

.. code-block:: shell

   sudo pip install parabam

This will also place an executable in a location on your path. So after typing this command you should be able to do the following:

.. code-block:: shell

   >>> parabam
   >>> parabam
       ----------------------------------------------------------------  

       About: 
           Parabam - analyse bam file in parallel       

       Usage:
           parabam <command> [options] instruction:{Python} input:{BAM} output:{BAM/CSV}       

       Command:

           stat     Genereate stats regarding the BAM file
           subset   Create a subsetted BAM file

If typing `parbam` into your terminal yields the result above, you have succesfully installed parabam.

The manual way
--------------

Some users may wish to install from source. The source may be [downloaded from here](https://github.com/jhrf/parabam).
This part of the documentation isn't written yet, but if you want to do this you probably know how to do it anyway.

The precompiled way
-------------------

If you really can't be bothered, then perhap you could try some of the precompiled binaries listed here. 

Simply copy these to a location on your path and type:

.. code-block:: shell
	parabam

It should work out of the box if you picked the right binary.

Running parabam on a cluster
++++++++++++++++++++++++++++

Some users may wish to run parabam on a high powered computing service like a cluster. On systems such as these you may not have root access, so you may not be able to install to the Python system directories. In these cases you may wish to use parabam within a [virtualenv](https://docs.python.org/3/tutorial/venv.html).

