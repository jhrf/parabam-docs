The `subset` interface
**********************

What is the subset interface for?
+++++++++++++++++++++++++++++++++

The `subset` operation allows the user to create BAMs containing a specific subset of reads from within a given BAM file. For example, a subset BAM containing all of the unmapped reads in the file, or all the reads which contain a certain sequence.

In this chapter we detail the process you must follow in order to run the subset operation.

Producing a single subset
+++++++++++++++++++++++++

With parabam it is easy to create BAM files containing a subset of reads from a larger BAM file. In this section we will explore how to create a single subset with parabam. With single subsets there is only one criterion by which reads are stored. This makes for simple code, but if you want to create multiple subsets you should read the section detailing :ref:`multiple subsets <multiple-section-ref>`.

From the command line
---------------------

Here is a run-though illustrating how we would create a parabam run that collects unmapped reads from a file.

In order to use the subset interface from the command line we first navigate to a directory where we will conduct our analysis.

.. code-block:: shell

   cd /analysis/parabam

We then make a new file which will contain the code which parabam will use to analyse the BAM file. Let's make our instructions file:

.. code-block:: shell

   touch get_unmapped_reads.py

Now we edit the file we just created - *get_unmapped_reads.py* - to include the following code.

.. code-block:: python

	def rule(read,constants,master):
		if read.is_unmapped:
			return True
		return False

By returning ``True`` we tell parabam to store the read in its subset.

We would then invoke this instruction from the command line as follows:

.. code-block:: shell

   parabam subset -p16 -c50000 -i get_unmapped_reads -b /path/to/bam/file_name.bam -o unmapped_reads

This will create a file called unmapped_reads.bam with all the unmapped reads from our BAM file.

Notice that we include the arguments ``-p`` and ``-c``. These options tell parabam how many processors to use and how many reads to hold in memory at any one time. The more processes you allow parabam to run, the quicker it can run on all the reads in a file. However, you should not specify more processes than your computer has processors. This will cause your computer to slow down. Additionally, the more reads that you allow parabam to store in memory, the faster it will run. But beware. Specifing too many reads will make your computer run out of memory and become unresponsive.

If you need to know what any of the arguments to the subset operation are just type the following to view the help text:

.. code-block:: shell

   parabam subset -h

Producing multiple subsets
++++++++++++++++++++++++++

Sometimes we may wish to store multiple subsets from the same BAM file. We can do this both on the command line or by invoking parabam programatically via Python.

From the command line
---------------------

This example will create two subsets. One subset of reads which are deemd secondary aligments and one subset which failed QC. We will make use of calls to variables defined in pysam.AlignedSegment. The full documentation for this class is listed in :doc:`Fundamentals <Fundamentals>`.

First, we navigate to a directory where we can carry out the analysis and create an instructions file:

.. code-block:: shell

	cd /analysis/parabam
	touch multiple_subsets.py

We then need to write the code that allows us to create the subsets. It's more involved than the single subset example above. We should edit the file multiple_subsets.py to contain the following code:

.. code-block:: python

	def get_subset_types():
		return ["failed_qc","secondary_aligments"]

	def rule(read,constants,master):
		belongs = []
		if read.is_qcfail:
			belongs.append("failed_qc")
		elif read.is_secondary:
			belongs.append("secondary_aligment")

		return belongs

The above code block sees us introduce the function ``get_subset_types``. Notice how first we define the subset groups as a list of strings. These strings can be anything you want, but it make sense to label them appropriately. 

Then, in the rule, we return a list of subsets to which the read belongs. For example, if a read returns ``True`` for both ``is_qcfail`` and ``is_secondary`` it will be added to both files.

We can then run parabam from the command line like so, imagining that we have a file `human_sample_001.bam`:

.. code-block:: shell
	
	parabam subset -v2 -i multiple_subsets -b /path/to/bam/human_sample_001.bam

This will produce two files. One file called ``human_sample_001_failed_qc.bam`` and the other called ``human_sample_001_secondary_aligments.bam``. Notice that parabam automatically handles the naming of these files.

Programatically
---------------

It is possible to integrate parabam subset into your program by making use of the interface class. 

Simply create a new object of type parabam.subset.Interface and call the run method. The specification follows.

Subset Specification
++++++++++++++++++++

Below is the parabam subset specification

.. automodule:: parabam.interface.subset
   :members:
