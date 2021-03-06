# Graph generator 
## Specify output size
- is [it](https://github.com/ldbc/ldbc_snb_datagen/wiki/Configuration#generator-parameters) enought? or it should be one parameter?
## Quick instruction
 - run once to build docker images: `./build_images.sh`
 - to generate data from generator `./generate_input_data.sh path/to/some_params.ini`
 - to convert them `./generate_output_data.sh`. Output data will be located in `output/merged` folder. 
 - to import data into neo4j:
 ```bash
 cd output/merged
 export NEO4J_HOME=...      # it should contain folder bin and neo4j-admin inside. Look into import.sh
 su neo4j                   # or run import.sh using sudo. 
 ./import.sh
 ```
Data would be imported to `graph.db` database inside local neo4j instalation.

## What is done
 - fix [converter](https://github.com/PlatformLab/ldbc-snb-impls)- version with fixes- [link]([converter](https://github.com/seleythen/ldbc-snb-impls)
 - created Dockerfile to create image with working [converter](https://github.com/seleythen/ldbc-snb-impls/tree/master/snb-interactive-neo4j)
 - create scripts to prepare output from generator and run converter
## Problems
 - strange column `creationDate` in files `social_network/dynamic` folder. This column (its placed as first, and being present in all files) cause most of problems with compatibility of old converter- so I decided to remove it. 
 - merged- new version make distinction between dynamic and static part. Imported separately (dynamic first)- failed. So I decided to merge them into one folder. Maybe loading static part first would also work.
 - some bugs in converter:
     - date- creation date has different format in new data
     - columns headers- to person table, during processing by converter, there was added new columns- `email` and `speaks`, but they are not present in header columns specification
     - add few columns to definition of tables in `converter`- they are hardcoded, their order also.