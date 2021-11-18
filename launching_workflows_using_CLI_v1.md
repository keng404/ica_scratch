To launch workflows in ICA programmatically via the CLI:

Below is one such command:

```ica workflows versions launch wfl.09a17485f67f4ef7b3dd87fa81ca9e01 DRAGEN_Enrichment_Built_In_Lic__4554923 --name DRAGEN_enrichment_launch_from_cli --input test.input.json```


There are a few things going on in the command line above, so I'll try to break it down below:


1. To create  test.input.json 
you will need to grab the input JSON from a successful pipeline run. First, you'll use the ```ica workflows runs list``` to grab workflow runs and their statuses. You should be able to use the timestamps to identify the pipeline run of interest. 

```ica workflows runs list```  will give you the workflow_id that you'll need in the ID column. 

Then use the command ica workflows runs get ${workflow_id} and copy the JSON from input field and paste it into a new file. You will need to modify the JSON by replacing the appropriate fields with GDS paths to your input.

2. Identifying the appropriate workflow version and workflow ID [wfl.09a17485f67f4ef7b3dd87fa81ca9e01 DRAGEN_Enrichment_Built_In_Lic__4554923]
In the output of  ```ica workflows runs get ${workflow_id}``` command, you will see a  workflowVersion.href  field as seen below :
```workflowVersion.href https://use1.platform.illumina.com/v1/workflows/wfl.09a17485f67f4ef7b3dd87fa81ca9e01/versions/DRAGEN_Enrichment_Built_In_Lic__4554923```

Alternatively, you could also use ```ica workflows list``` and grab the workflow_id (1st column) and workflow version (2nd column)

3. Pipeline Run name (--name)
pick any name you'd like 👍 . By default, ICA will generate a random name if you leave this field blank.

# markmap

## Links

- <https://markmap.js.org/>
- [GitHub](https://github.com/gera2ld/markmap)

## Related

- [coc-markmap](https://github.com/gera2ld/coc-markmap)
- [gatsby-remark-markmap](https://github.com/gera2ld/gatsby-remark-markmap)

## Features

- links
- **inline** ~~text~~ *styles*
- multiline
  text
- `inline code`
-
    ```js
    console.log('code block');
    ```
- Katex - $x = {-b \pm \sqrt{b^2-4ac} \over 2a}$

