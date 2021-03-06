# spaTyper.py: Generate spa type identification
[![DOI](https://zenodo.org/badge/258175615.svg)](https://zenodo.org/badge/latestdoi/258175615)



Version: 0.3.3
```
USAGE: spaTyper [-h] [-r REPEAT_FILE] [-o REPEAT_ORDER_FILE] [-d FOLDER] [-f FASTA [FASTA ...]] [-g GLOB] [--output OUTPUT] [-e] [--version] [--debug]
```

Prints spa type to stdout - egenomics letter combination and then the ridom spa type.
If multiple pcr products are found will print spa types for each product.
It downloads sparepeats.fasta and spatypes.txt from the ridom server to repository directory if files not provided or already in directory.

```
optional arguments:
-h, --help            show this help message and exit
-r REPEAT_FILE, --repeat_file REPEAT_FILE List of spa repeats (http://spa.ridom.de/dynamic/sparepeats.fasta)
-o REPEAT_ORDER_FILE, --repeat_order_file REPEAT_ORDER_FILE List spa types and order of repeats (http://spa.ridom.de/dynamic/spatypes.txt)
-d FOLDER, --folder FOLDER Folder to save downloaded files from Ridom/Spa server
-f FASTA [FASTA ...], --fasta FASTA [FASTA ...] List of one or more fasta files.
-g GLOB, --glob GLOB  Uses unix style pathname expansion to run spa typing on all files. If your shell autoexpands wildcards use -f.
-e, --do_enrich       Do PCR product enrichment. [Default: False]
--output OUTPUT	Provide an output file or print by default using standard out.
--version             show program's version number and exit
--debug               Developer messages
```

## Installation
It requires python 3. Install it using pip package (https://pypi.org/project/spaTyper/)
```
pip install spaTyper
```
or clone the repository and install it using setup.py
```
git clone https://github.com/JFsanchezherrero/spa_typing.git
cd spa_typing
pip install setup.py
```

## How it works

Given a fasta file or multiple fasta files, this script identifies the repeats and the order and generates a spa type.

The repeat sequences and repeat orders found on http://spaserver2.ridom.de/ are used to identify the spa type of each enriched sequence.

Ridom spa type and the egenomics repeat sequence are then reported back to the user.

If enriched option provided, the script searches for 50bp to 5000bp sequences produced by the following primer sets
```
TAAAGACGATCCTTCGGTGAG, CAGCAGTAGTGCCGTTTGCTT
AGACGATCCTTCGGTGAGC, GCTTTTGCAATGTCATTTACTG
ATAGCGTGATTTTGCGGTT, CTAAATATAAATAATGTTGTCACTTGGA
CAACGCAATGGTTTCATCCA, GCTTTTGCAATGTCATTTACTG
```

If an enriched sequence is found by a primer set, subsequent primer sets are not used.

## Load it as a module
This scripts can be loaded and installed as a python module. Python 3 version only.

```
	import spaTyper
	
	## download file repeats   
	repeat_file = spaTyper.utils.download_file_repeats(folder, False)
	
	## download file repeats   
	repeat_order_file = spaTyper.utils.download_file_types(folder, False)
	
	## Get the SpaTypes in fasta sequences
	seqDict, letDict, typeDict, seqLengths = spaTyper.spa_typing.getSpaTypes(repeat_file, repeat_order_file, False)
	
	## read fasta file
	fasta_file = "my_genome.fasta"
	qDict = spaTyper.utils.fasta_dict(fasta_file)
	
	## find pattern
	for i in qDict.keys():
		pattern = spaTyper.spa_typing.findPattern(qDict[i], seqDict, seqLengths, debug)
		if pattern:
			if j in pattern.keys():
				splitted = pattern[j].split('::')
				print("Sequence name: ",j, "Repeats:", splitted[2], "Repeat Type:", splitted[1], '\n')    
```

## Copyright
Original code written by mjsull (https://github.com/mjsull/spa_typing)

Jose F. Sanchez-Herrero updated the code, change to python3, and set to use it as a module

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4063625.svg)](https://doi.org/10.5281/zenodo.4063625)


