# ica_scratch

## SCENARIO #1: An Example of how to port WDL workflows on ICA

- used WDL workflows from [stjudecloud](https://github.com/stjudecloud/workflows)
- Chose workflow bam-to-fastq.wdl that references a few other wdl recipes
- [Created Docker image](https://git.illumina.com/keng/ica_scratch/blob/master/wdl_test/Dockerfile) with all binaries (i.e. samtools, fq, picard) of interest
- Installed Cromwell to run WDL workflow
- Created options JSON file [here](https://git.illumina.com/keng/ica_scratch/blob/master/wdl_test/output_directory.json)
```bash
# Key parameter in the JSON is: 
"use_relative_output_paths": true
```
- Wrapper script that initializes this workflow can be found [here](https://git.illumina.com/keng/ica_scratch/blob/master/wdl_test/bam_to_fastq_wdl_wrapper.py)

## SCENARIO #2: An Example of how to port Nextflow workflows on ICA

- used nextflow pipeline [hlatyping](https://github.com/nf-core/hlatyping)
- [Docker image](https://git.illumina.com/keng/ica_scratch/blob/master/nextflow_test/Dockerfile) you base your nextflow-based tool would need nextflow to run the pipeline and have the binaries of interest installed. Many nextflow pipelines have a conda profile to install all binaries of interest with a single line of code.
- main thing is to check out each process defined in nextflow scripts ( e.g. usually files with '.nf' extension) and make sure the parameter **publishDir** is defined. This will allow ICA to collect the results of any process of interest. For Example:
```bash
process < name > {
    publishDir "", mode: copy
   [ directives ]

   input:
    < process inputs >

   output:
    < process outputs >

   [script|shell|exec]:
   < user script to be executed >

}
```
- You will need to run **nextflow clean -f** to remove cache and work directories of a pipeline run. There are symlinked files which makes the folder/file-syncing of ICA to fail.
- See a simple wrapper script [here](https://git.illumina.com/keng/ica_scratch/blob/master/nextflow_test/tool_wrapper_nf.py). Nextflow is invoked like so:
```
nextflow run /opt/hlatyping/main.nf -profile conda--input ${INPUT_DIR}/${STRING_TO_GLOB_FASTQs} --output_dir ${OUTPUT_PATH} -work-dir ${WORKDIR_PATH}
```
Notes: ${WORKDIR_PATH} and ${OUTPUT_PATH} should be different directories to prevent causing ICA to copy intermediate files and cause UI issues. The CWL of the tool is provided [here](https://git.illumina.com/keng/ica_scratch/blob/master/nextflow_test/hlatyping/hlatyping.cwl)

## SCENARIO #3: What if a customer wants to override a DRAGEN tool/workflow?  Will mainly be a workaround

- You can override the command run inside the DRAGEN Docker image to run a user-defined workflow. See wrapper [here](https://git.illumina.com/keng/ica_scratch/blob/master/joint_genotype_mod/joint-genotyping-new.cwl)

## SCENARIO #4: I only use Singularity images. How would you make Docker containers?
- See this document for a quick [recipe](https://git.illumina.com/keng/ica_scratch/blob/master/converting_singularity_to_docker.txt) to convert Singularity images into Docker images
- Our main recommendation is to use the Singularity recipe file ( i.e. SIF file for all recipes developed in singularity >= 3.0) and use the table [here](https://sylabs.io/guides/3.5/user-guide/singularity_and_docker.html#sec-deffile-vs-dockerfile) to create a Dockerfile. However if needed the recipe provided in the previous point will help

## Items to [add](https://git.illumina.com/keng/ica_scratch/blob/master/to_add.md)
