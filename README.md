# bidsphysio
Converts physio data (CMRR, AcqKnowledge, Siemens PMU) to BIDS physiological recording

[![Docker image](https://img.shields.io/badge/docker-cbinyu/bidsphysio:latest-brightgreen.svg?logo=docker&style=flat)](https://hub.docker.com/r/cbinyu/bidsphysio/tags/)
[![TravisCI](https://travis-ci.com/cbinyu/bidsphysio.svg?branch=master)](https://travis-ci.com/cbinyu/bidsphysio)
[![CodeCoverage](https://codecov.io/gh/cbinyu/bidsphysio/branch/master/graph/badge.svg)](https://codecov.io/gh/cbinyu/bidsphysio)

## Usage
```
dcm2bidsphysio --infile <DCMfile> --bidsprefix <Prefix>
acq2bidsphysio --infile <acqfile> --bidsprefix <Prefix>
pmu2bidsphysio --infile <acqfiles> --bidsprefix <Prefix>
```

Example:
```
dcm2bidsphysio --infile 07+epi_MB4_2mm_Physio+00001.dcm      \
                           --bidsprefix BIDSfolder/sub-01/func/sub-01_task-REST_run-1
```

## Arguments
 * `<DCMfile>` is the DICOM file that contains the physiological recordings.
 * `<acqfile>` is an AcqKnowledge file with physiological recordings.
 * `<Prefix>` is the prefix that will be used for the BIDS physiology files.  If all physiological recordings have the same sampling rate and starting time, the script will save the files: `<Prefix>_physio.json` and `<Prefix>_physio.tsv.gz`.  If the physiological signals have different sampling rates and/or starting times, the script will save the files: `<Prefix>_recording-<label>_physio.json` and `<Prefix>_recording-<label>_physio.tsv.gz`, with the corresponding labels (e.g., `cardiac`, `respiratory`, etc.).

Note: If desired, you can use the corresponding `_bold.nii.gz` BIDS file as `--bidsprefix`. The script will strip the `_bold.nii.gz` part from the filename and use what is left as `<Prefix>`. This way, you can assure that the output physiology files match the `_bold.nii.gz` file for which they are intended.

## Installation
You can install the tool by downloading the package and installing it
with `pip`:

```
mkdir /tmp/bidsphysio && \
    curl -sSL https://github.com/cbinyu/bidsphysio/archive/master.tar.gz \
        | tar -vxz -C /tmp/bidsphysio --strip-components=1 && \
    cd /tmp/bidsphysio && \
    pip install . && \
    cd / && \
    rm -rf /tmp/bidsphysio
```

## Docker usage
```
docker run [docker options] cbinyu/bidsphysio \
    --infile <DCMfile/AcqFile>      \
    --bidsprefix <Prefix>
```
Note that the `<DCMfile>` or `<AcqFile>` and the `<Prefix>` should use the corresponding paths inside the Docker container.

Example:
```
docker run --rm \
    --user $(id -u):$(id -g) \
    --name test_bidsphysio \
    --volume /tmp:/tmp \
    cbinyu/bidsphysio \
        --infile /tmp/07+epi_MB4_2mm_Physio+00001.dcm \
        --bidsprefix /tmp/test
```


## How to use in your own Python program
After installing the module using `pip` (see [above](https://github.com/cbinyu/bidsphysio#installation "Installation") ), you can use it in your own Python program this way:
```
from bidsphysio import bidsphysio
bidsphysio.dcm2bids( dicom_file,  prefix )
bidsphysio.acq2bids( acq_file,    prefix )
bidsphysio.pmu2bids( [acq_files], prefix )
```
