# bidsphysio

This is a modified version of *bidsphysio* package. The original package is accssible [here](https://github.com/cbinyu/bidsphysio).

Converts physio data (CMRR, AcqKnowledge, Siemens PMU) to BIDS physiological recording

[![DOI](https://zenodo.org/badge/239006399.svg)](https://zenodo.org/badge/latestdoi/239006399)


Note: If desired, you can use the corresponding `_bold.nii.gz` BIDS file as `--bidsprefix`. The script will strip the `_bold.nii.gz` part from the filename and use what is left as `<Prefix>`. This way, you can assure that the output physiology files match the `_bold.nii.gz` file for which they are intended.

## Installation
This is a modified version of the *bidsphysio* package.

You can install this version with `pip`:
```
mkdir /tmp/bidsphysio && \
    curl -sSL https://github.com/rezashr/bidsphysio/archive/master.tar.gz \
        | tar -vxz -C /tmp/bidsphysio --strip-components=1 && \
    cd /tmp/bidsphysio && \
    pip install . && \
    cd / && \
    rm -rf /tmp/bidsphysio
```

```
dcm2bidsphysio --infile <DCMfile> --bidsprefix <Prefix>
acq2bidsphysio --infile <acqfiles> --bidsprefix <Prefix>
pmu2bidsphysio --infile <pmufiles> --bidsprefix <Prefix> --dicom <DICOM DIR>
edf2bidsphysio --infile <edffile> --bidsprefix <Prefix>
```

### Arguments
 * `<physiofiles>` space-separated files with the physiological recordings.

    Supported file types:
	 * `<.dcm>` or `<.log>`: DICOM or log file with physiological recording from a CMRR
     Multi-Band sequence (only a single file for `<.dcm>`).
	 * `<.acq>`: AcqKnowledge file (BioPac).
	 * `<.puls>` or `<.resp>`: Siemens PMU file (VB15A, VBX, VE11C).
	 * `<.edf>` SR Research .edf file
 * `<Prefix>` is the prefix that will be used for the BIDS physiology files.  If all physiological recordings have the same sampling rate and starting time, the script will save the files: `<Prefix>_physio.json` and `<Prefix>_physio.tsv.gz`.  If the physiological signals have different sampling rates and/or starting times, the script will save the files: `<Prefix>_recording-<label>_physio.json` and `<Prefix>_recording-<label>_physio.tsv.gz`, with the corresponding labels (e.g., `cardiac`, `respiratory`, etc.).
 * `<DICOM DIR>` is the dicom directory of the functional images.
 * `--verbose` will print out some warning messages.
