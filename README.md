## Intel Optimized GATK-3-4 Germline SNPs and Indels Variant Calling Workflow. 

### WORKFLOWS AND JSONS
This repository contains a few different files - each tuned for certain requirements. 

├── 2T\_PairedSingleSampleWf\_optimized.inputs.json *&rarr;* Throughput JSON file \
├── 56T\_PairedSingleSampleWf\_optimized.inputs.20k.json *&rarr;* 20k test data JSON file \
├── 56T\_PairedSingleSampleWf\_optimized.inputs.json *&rarr;* Latency JSON file \
├── HDD\_2T\_PairedSingleSampleWf\_optimized.inputs.json *&rarr;* Throughput JSON file (for HDD) \
├── HDD\_56T\_PairedSingleSampleWf\_optimized.inputs.json *&rarr;* Latency JSON file (for HDD) \
├── PairedSingleSampleWf\_noqc\_nocram\_optimized.wdl *&rarr;* WDL optimized for on-prem \
├── PairedSingleSampleWf\_noqc\_nocram\_withcleanup\_optimized.wdl *&rarr;* WDL optimized for on-prem with cleanup of output results (for throughput analysis)

### DATASETS
Contact Intel/Broad for access to the WGS data needed for this workflow.

### TOOLS
For on-prem, the workflow uses non-dockerized tools. To keep up with the exact 
versions released by Broad for their best practices workflow, we download the 
tools from the docker image to our shared file system. 

Run the command: 
```
docker run -v /path/to/shared_filesystem:/path/to/shared_filesystem -it broadinstitute/genomes-in-the-cloud:2.3.1-1504795437 /bin/bash
```

This command will pull the docker image (if it is not already there locally), 
and put you within the container from where you can copy the tools needed for 
the workflow. 

```
root@54754360159e:/usr/gitc# cp /usr/local/bin/samtools gatk4 bwa picard.jar /path/to/shared_filesystem
root@54754360159e:/usr/gitc# exit
```

In addition to above, the Hybrid workflow uses the latest optimized GATK3.8 jar 
with AVX-512 optimizations and other optimizations which can be obtained from 
GATK website. 

##placeholder for GATK 3.8 relaese

Lastly, Hybrid workflow also needs a tool called "VerifyBamID" that can be 
downloaded as follows: 

* If on Centos, you will need to do a `yum install curl-devel` before proceeding. 
```
cd /path/to/shared_filesystem
wget https://github.com/Griffan/VerifyBamID/archive/c8a66425c312e5f8be46ab0c41f8d7a1942b6e16.zip && \
unzip c8a66425c312e5f8be46ab0c41f8d7a1942b6e16.zip && \
cd VerifyBamID-c8a66425c312e5f8be46ab0c41f8d7a1942b6e16 && \
mkdir build && \
cd build && \
CC=$(which gcc-4.9) CXX=$(which g++-4.9) cmake ..  && \
make && \
make test && \
cd ../../ && \
mv VerifyBamID-c8a66425c312e5f8be46ab0c41f8d7a1942b6e16/bin/VerifyBamID . && \
rm -rf c8a66425c312e5f8be46ab0c41f8d7a1942b6e16.zip VerifyBamID-c8a66425c312e5f8be46ab0c41f8d7a1942b6e16
```
