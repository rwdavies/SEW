SEW
===
**__Current Version: pre-release__**
Release date: Future

[![Build Status](https://travis-ci.org/rwdavies/SEW.svg)](https://travis-ci.org/rwdavies/SEW)

Changes in latest version

1. Initial release

For details of past changes please see [CHANGELOG](CHANGELOG.md).

SEW is an R program for reference panel free, long read phasing. SEW runs on a single sample given sequencing reads in BAM format, as well as a list of positions to phase, and outputs imputed genotypes in VCF format.

## Installation and quick start on real data example

### Quick start on Linux and Mac

bioconda installation forthcoming. Otherwise, SEW can be installed in a few ways. The simplest way to get a release is as follows. First, install R. Then, do the following 
```
git clone --recursive https://github.com/rwdavies/SEW.git
cd SEW
./scripts/install-dependencies.sh
cd releases
wget https://github.com/rwdavies/sew/releases/download/1.0.0/SEW_1.0.0.tar.gz ## or curl -O
R CMD INSTALL SEW_1.0.0.tar.gz
```

A quick test (using simulated data) can be performed using 
```
wget https://www.well.ox.ac.uk/~rwdavies/ancillary/SEW_example_2019_01_11.tgz ## or curl -O
tar -xzvf SEW_example_2019_01_11.tgz
./SEW.R --chr=10 --bamlist=bamlist.txt --posfile=posfile.txt --phasefile=phasefile.txt --outputdir=./
```

To install the latest development code in the repository, use `./scripts/build-and-install.sh`. To install alternative releases, download the releases from Github, and install using `R CMD INSTALL` or similar.

If you encounter problems during installation, please consult the [STITCH](https://github.com/rwdavies/STITCH) website, or raise an issue.

A bioconda installation is forthcoming. Installation details will appear here when this is available.

## Help, command line interface and common options

For a full list of options, in R, query `?SEW`, or from the command line, `SEW --help`. For a brief writeup of commonly used variables, see [Options.md](Options.md). To pass vectors using the command line, do something like `SEW.R --unwindwIterations='c(30,60)'`.

## License

Forthcoming

## About heuristics

SEW features an expectation-maximization (EM) algorithm that iteratively tries to 1) determine the best parameters of the model given probabilistic read partitions and 2) probabilistically partition reads into two haplotypes given parameters of the model. The EM algorithm is excellent at finding local minima, but relies on initial parameter estimates, and will have difficulty getting to global maxima.

To help overcome this, SEW uses a heuristic. More detail is available in the paper, but briefly, these local, but not global, maxima often reflect where there is good local reconstruction of the two haplotypse, but there is a switchover at some point between the estiamted computational haplotypes and the true haplotypes. The heuristic will, on specified iterations (`unwindIterations`), check between every pair of SNPs, whether flipping the computational haplotypes at the second SNP and beyond and running the EM algorithm for a few iterations will improve the local fit of the model versus the current configuration.

The current heuristics were developed for use with high coverage long read data. Different depths or length of reads might work best with alternative heuristic settings. For advice, please feel free to open an issue or send an email. 

## Testing

Tests in SEW are split into unit or acceptance run using ```./scripts/test-unit.sh``` and ```./scripts/test-acceptance.sh```. To run all tests use ```./scripts/all-tests.sh```, which also builds and installs a release version of SEW. To make compilation go faster do something like ```export MAKE="make -j 8"```.

## Citation

Forthcoming

## Contact and bug reports

The best way to report bugs or to get help is to open an issue on Github. Alternatively, for more detail questions or other concerns please contact Robert Davies robertwilliamdavies@gmail.com