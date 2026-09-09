# Week 1: System Setup

## AI ready Code editor

I used Visual Studio Code with Remote SSH.

## Samtools version

I checked the version of `samtools` in the `bioinfo` environment with:

```bash
pixi run -m "$HOME/edu/bioinfo" samtools --version
```

Output:

```text
samtools 1.24
Using htslib 1.24
```

## Creating a nested directory structure

From the `week01` directory, I created directories for raw data, results, workflow files, and analysis code. The results directory contains separate subdirectories for figures and tables.

```bash
mkdir -p rawdata results/{figures,tables} workflow code
```

I displayed the directory structure with:

```bash
tree
```

Output:

```text
.
├── code
├── rawdata
├── README.md
├── results
│   ├── figures
│   └── tables
└── workflow
```

## Creating files in different directories

I created a sample sheet in the data directory and a download script in the code directory:

```bash
touch rawdata/samples.tsv
touch code/download.sh
```

## Accessing files using relative and absolute paths

### Relative path

While working inside the `week01` directory, I accessed the sample information file using a relative path:

```bash
ls rawdata/samples.tsv
```

Output:

```text
rawdata/samples.tsv
```

### Absolute path

I accessed the file using its absolute path:

```bash
ls /home/zhiyi/1data/projects/09.07.26_zhiyi_BMMB852_1/week01/data/samples.tsv
```

Output:

```text
/home/zhiyi/1data/projects/09.07.26_zhiyi_BMMB852_1/week01/data/samples.tsv
```

## Final directory structure

```text
week01
├── code
│   └── download.sh
├── rawdata
│   └── samples.tsv
├── README.md
├── results
│   ├── figures
│   └── tables
└── workflow
```

## Version control

Commit the files and pushed them to my public GitHub repository.

```bash
cd ..
git add week01
git commit -m "Complete Week 1 assignment"
git push -u origin main
```
