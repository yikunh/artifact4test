# README #
Binary Similarity Manual

### Directory Tree ###
```
binsim/
`-- jaccardIndex1			# executable that calculates similarity scores
`-- qemu-x86_64				# executable that generates function signatures
`-- analyse_time_file.txt	# txt that records the time cost for each signature generation
`-- random.txt				# txt that stores function parameters
`-- run-qemu-analysis.py	# script to get function signatures
`-- run-jaccard-x64.py		# script to generate similarity report
`-- test-binary/			# stores the binary pairs to test
`-- ...						
```


### How To Analysis ###
We have already prepared data produced by ida pro.<br>
1. To get the function signatures of the programs:
	```bash
	python3 run-qemu-analysis-x64.py programname
	# e.g. python3 run-qemu-analysis-x64.py x64_wget_1_20_3_gcc75_O0
	```
	The result will be stored in ./programname/SigExtracted

2. If you want to get the signature of a specific function
	```bash
	./qemu-x86_64 programname functionname
	```
	The result will be stored in ./programname/SigExtracted

3. To compare the signatures between programs, create a directory first
	```
	foldername/
	`-- log
	```
	and then run
	```bash
	python3 run-jaccard-x64.py programname_O3 programmename_O0 foldername
	```
4. To compare a specific function in functionA against functions in programB, create a directory first
	```
	foldername/
	`-- log
	```
	and then run
	```bash
	python3 run-jaccard-x64.py programname_O3 programmename_O0 foldername functionname
	```
	
### How To read the result ###
You will get the similarity report under the directory you have specified.
```
folder/
`-- log/
`-- distinguished_log.txt
`-- error_result_log.txt
`-- result_log.txt
`-- routine_time_log.txt
```
1. result_log.txt

	This file records the similarity score between the same function but from differenct elfs(e.g. function acpt_write from x64_openssl_1_1_1g_gcc75_O0 and x64_openssl_1_1_1g_gcc75_O3)<br>
	At the end of this file, an overall summary is recorded, which includes the number of functions in both programs, the correct number, precision and rankX rate(explained in distinguish_log.txt). 

2. error_result_log.txt

	This file records the false possitives and their corresponding similarity scores

3. distinguish_log.txt

	This file records the true possitives.For each function in programA, we record all the functions in functionB that has the same similarities(also the highest) in this file. We calculate rankX,  the ratio of function pairs among all the true possitives where functionA has less than X most similar functions in programB.

4. routine_time_log.txt

	This file records the time cost of comparing each function in programA with all the functions in programB

5. log/

	This directory stores the similarity scores between each function in programA and all the functions in programB.