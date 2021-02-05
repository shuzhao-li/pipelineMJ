# pipelineMJ - Pipeline for metabolomics at Jackson Laboratory

Data pipeline for metabolomics, being developed as
- templates in Jupyter notebooks
- automated workflow 

We use multiple tools in different computer languages. 
The individual tools are maintained/developed in their own repositories.
This repository focuses on the pipleline with a set of defined goals.

## Data Preprocessing

### Conversion of raw data to open formats

This can be done using manufacturers' tools, or MSconvert (part of http://proteowizard.sourceforge.net/download.html), while we recommend ThermoRawFileParser (https://github.com/compomics/ThermoRawFileParser).

Centroiding m/z peaks is often done in this step. E.g.

    mono ThermoRawFileParser.exe -d=/home/user/data_input/

Centroiding is default. Use `-p, --noPeakPicking` to keep profile data.


### Chromatographic peak detection 

In LC-MS data, peaks are 3-D. People often refer "peak picking" to finding peaks in the m/z axis. When centroiding is performed on the data, it is collapsing each m/z peak into a single representative value, thus equivalent to "peak picking". 

When working with centroid data, the peak detection will be mainly on the axis of retention time, i.e. chromatograms.

- Using XCMS

the function is `findChromPeaks`:

    cwp <- CentWaveParam(peakwidth = c(2, 30), 
            ppm = 1,  noise = 5000, prefilter = c(6, 5000), 
            fitgauss=FALSE, verboseColumns=FALSE)

    xdata <- findChromPeaks(raw_data, param = cwp)

The parameters are critical. One should spend time to inspect a dataset to decide on the paramters.

Multiple works try to determine the optimal parameters automatically.
Discussions are ongoing.


### Alignment and correspondence

In high-res data, alignment of m/z is straight forward. 
Retention time alignment is usually critical, especially for large samplesets.

Alignment across samples (correspondence) is where many problems occur.



## Annotation 

- grouping ions
- computational annotation
- identification, including matching to libraries of authentic compounds, and MS/MS.

We are using "Empirical Compound" to field the result of annotation, and as entry point to the next version of mummichog.

An "Empirical Compound" has
    - neutral_base_mass
    - experiment_belonged
    - annotation_method
    - associated MS1 and MSn spectra
and 
    identity_probability = {
                  # (compound or mixtures): probability
                  (Compound x): 0.0,
                  (Compound y, Compound z): 0.0,
          }

The detailed code is at
https://github.com/shuzhao-li/metDataModel


## Quality control

Both before and after pre-proccessing


## Batch correction, normalization


## Statistical analysis

Follow good practice in statistics.
Check data distribution.
If the data contain many missing values, choose statistical approach accordingly.

- MetaboAnalyst (https://www.metaboanalyst.ca/) can do most of these if you like clicking through webpages.

## MWAS

## Pathway analysis

The mummichog software is embedded in
- MetaboAnalyst (https://www.metaboanalyst.ca/)
- XCMS Online (https://xcmsonline.scripps.edu/)

Mummichog version 3 is expected to go public in Summer 2021.


## Integration with other data types

Pls check out Day 5 materials in https://github.com/shuzhao-li/espol-workshop.

Related projects:
- https://github.com/shuzhao-li/hiconet
- https://www.omicsnet.ca/


## LC-MS/MS

To add.




## To start a Jupyter Notebook via Docker:

`docker run -v /Users/shuzhao/w:/home/jovyan -p 8888:8888 jupyter/scipy-notebook`

or 

`docker run -v /Users/shuzhao/w:/home/jovyan -p 8888:8888 jupyter/r-notebook`

The above examples map the notebook server to the local directory `/Users/shuzhao/w`.

One can of course install the tools locally without using Docker.