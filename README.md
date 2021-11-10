# ICA User Guide --- pothole avoidance

## Docker

### Dockerfile recommendations
- Find code for tool/pipeline on GitHub ---- there may already be a Dockerfile present in the repository
- Tool/pipeline may already be on DockerHub ; built by the group/individuals maintaining the tool or by someone in the community
- Find compiled binary of tool and wget/curl this binary in your Dockerfile or copy the binary locally to be copied into Docker image during build
- Follow readme instructions and copy those instructions as a RUN command in your Dockerfile
- use pip or conda to do installation ---- this may not always be recommended as there may be breaks in your build as conda gets updated

### Integrating Docker containers to ICA
- having the Docker image available on DockerHub/Quay
- use ```bash docker save -o ${docker_image_name}.tar ${docker_image_name} ``` and transfer this TAR file to ICA via the CLI/ ICA connector
  - Once this TAR file is uploaded on ICA, you will need to change the file's format to Docker and add this Docker image to the Docker repository
- Using an AWS ECR ...
- private Docker repositories


## CWL

### crafting/developing your own CWL recipes
- Use the menu tabs when creating a new tool in ICA
- Useful guides/tips for building a CWL from scratch
- Helpful CWL recipes from the community
- Helpful internal CWL recipes showcasing some best practices

### debugging your CWL
- add $(runtime.outdir) as an output to help troubleshoot/debug a CWL  as you develop it
- Do so offline
- Create dummy JSON/YAML input file to test compatiblity
- Use cwltool to 'test' your CWL --- echo your command line in a basic base Docker image to test and not run your tool

## ICA pipelines

### ICA pipeline best practices
- try to version control your tools/pipelines by updating the version setting in the ICA UI
- make sure the name of your pipeline is different from any underlying tool within a pipeline
- clone a working tool/pipeline and edit as you wish
- ensure the multivalue setting or single value setting of an input/output file node matches each input/output node of a tool
- Use parameter settings for configurable parameters in your pipeline/tool and unlock tehm

### troubleshooting pipeline runs
- logs of interest ( how to access these)
   - ```ica files list gds://${workflow_id} | grep task | grep log```
   - ```ica tasks runs get ${task_id}```
   - ```ica workflows runs get ${workflow_id} --include definition | grep definition | awk '{split($2,a,"\\|"); print a[3]}' |  base64 -d - > troubleshoot.zip```
      - unzip this troubshoot.zip file and you'll get a CWL of your initial pipeline and what gets translated to ICA
- wrapper script to capture stdout/stderr independent of ICA

### launching piplelines programmatically via the [CLI](https://github.com/keng404/ica_scratch/blob/master/launching_workflows_using_CLI_v1.md)

## demo code and CWL recipes for ICA v1
- BWA-mem CWL [example](https://github.com/keng404/ica_scratch)
- CWL-wrapped nextflow [example](https://github.com/keng404/ica_scratch)
- CWL-wrapped WDL [example](https://github.com/keng404/ica_scratch)

