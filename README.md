# docker-cwllog-generator
This program collects the cwl workflow (https://github.com/pitagora-galaxy/cwl) execution metrics from the workflow stderr, and saves the metrics to the JSON log file.

Dockerhub site: https://hub.docker.com/r/yyabuki/docker-cwllog-generator/


#Prerequisite:
  1) The cwl workflow should be executed with following four arguments:

    --debug
    --leave-container
    --compute-checksum
    --timestamps 

    e.g.
    % cwltool --debug --leave-container --compute-checksum --timestamps hisat2-se.cwl hisat2-se.yaml 2> stderr.log

  2) And following two files are needed.

    % docker info > docker_info
    % docker ps -a --no-trunc > docker_ps

#command:

    % docker run --rm \
        -v /path/to/result_dir:/cwl/result \
        -v /path/to/cwl_log_dir:/cwl/log \
        -v /path/to/docker_info_dir:/cwl/docker_info \
        -v /path/to/docker_ps_dir:/cwl/docker_ps \
        -v /path/to/input_dir:/cwl/input \
        yyabuki/docker-cwllog-generator cwl_log_generator.py \
        --docker_ps /cwl/docker_ps/'docker ps file' \
        --docker_info /cwl/docker_info/'docker info file' \
        --cwl_log /cwl/log/'cwl log file' \
        --cwl_input /cwl/input/'******.yaml'

    e.g.
    % docker run --rm \
        -v /work/hisat2/result:/cwl/result \
        -v /work/hisat2/cwl_log_dir:/cwl/log \
        -v /work/hisat2/src/input_dir:/cwl/input \
        yyabuki/docker-cwllog-generator cwl_log_generator.py \
        --docker_ps /cwl/log/docker_ps.txt \
        --docker_info /cwl/log/docker_info.txt \
        --cwl_log /cwl/log/cwl_stderr.log \
        --cwl_input /cwl/input/hisat2-se.yaml

    * /cwl/result volume must be specified.

=How to mount volumes=

    -v /path/to/result_dir:/cwl/result (required)
        /path/to/result_dir -- the directory path in which the cwl execution results are stored.

    -v /path/to/cwl_log_dir:/cwl/log
        /path/to/cwl_log_dir -- the directory path for storing the log file in which the cwl execution stderr is written.

    -v /path/to/input_dir:/cwl/input
        /path/to/yaml_dir -- the directory path for storing the yaml file required for executing the cwl.

    -v /path/to/docker_ps_dir:/cwl/docker_ps
        /path/to/docker_ps_dir -- the directory path for storing the file in which the docker ps results are written.

    -v /path/to/docker_info_dir:/cwl/docker_info
        /path/to/docker_info_dir -- the directory path for storing the file in which the docker info results are written.
