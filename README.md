# dcm2bidsphysio
Converts physio data from a CMRR DICOM file to BIDS physiological recording

## Usage
```
python dcm2bidsphysio.py --infile <DCMfile>      \
                         --bidsprefix <Prefix>
```

Example:
```
python dcm2bidsphysio.py --infile 07+epi_MB4_2mm_Physio+00001.dcm      \
                         --bidsprefix BIDSfolder/sub-01/func/sub-01_task-REST_run-1
```


## Docker usage
```
docker run [docker options] cbinyu/dcm2bidsphysio \
    --infile <DCMfile>      \
    --bidsprefix <Prefix>
```
Note that the `<DCMfile>` and the `<Prefix>` should use the corresponding paths inside the Docker container.

Example:
```
docker run --rm \
    --user $(id -u):$(id -g) \
    --name test_dcm2bidsphysio \
    --volume /tmp:/tmp \
    cbinyu/dcm2bidsphysio \
        --infile /tmp/07+epi_MB4_2mm_Physio+00001.dcm \
        --bidsprefix /tmp/test
```


## Arguments
 * `<DCMfile>` is the DICOM file that contains the physiological recordings.
 * `<Prefix>` is the prefix that will be used for the BIDS physiology files.  The script will save the files: `<Prefix>_recording-<label>_physio.json` and `<Prefix>_recording-<label>_physio.tsv.gz`, with labels `cardiac` and `respiratory` (if present in the `DCMfile`).

Note: If desired, you can use the corresponding `_bold.nii.gz` BIDS file as `--bidsprefix`. The script will strip the `_bold.nii.gz` part from the filename and use what is left as `<Prefix>`. This way, you can assure that the output physiology files match the `_bold.nii.gz` file for which they are intended.

