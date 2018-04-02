# docker-cwllog-generator
This program collects the cwl workflow (https://github.com/pitagora-galaxy/cwl) execution metrics from the workflow stderr, and saves the metrics to the JSON log file.

dockerhub site: https://github.com/pitagora-galaxy/cwl/docker-cwllog-generator

docker run --rm \
    -v /path/to/cwl_log_dir:/cwl/log \
    -v /path/to/yaml_dir:/cwl/yaml \
    -v /path/to/result_dir:/cwl/result \
    yyabuki/docker-cwllog-generator cwl_log_generator.py \
    --docker_ps /cwl/log/'docker ps file' \
    --docker_info /cwl/log/'docker info file' \
    --cwl_log /cwl/log/'cwl log file' \
    --cwl_input /cwl/src/'******.yaml'

e.g.
docker run --rm \
    -v /work/hisat2/cwl_log_dir:/cwl/log \
    -v /work/hisat2/src/yaml_dir:/cwl/yaml \
    -v /work/hisat2/result:/cwl/result \
    yyabuki/docker-cwllog-generator cwl_log_generator.py \
    --docker_ps /cwl/log/docker_ps.txt \
    --docker_info /cwl/log/docker_info.txt \
    --cwl_log /cwl/log/cwl_stderr.log \
    --cwl_input /cwl/src/hisat2-se.yaml

-How to mount volumes-

  -v /path/to/cwl_log_dir:/cwl/log
    /path/to/cwl_log_dir -- the directory path for storing the log file in which the cwl execution stderr is written.

  -v /path/to/yaml_dir:/cwl/yaml
    /path/to/yaml_dir -- the directory path for storing the yaml file required for executing the cwl.

  -v /path/to/result_dir:/cwl/result
    /path/to/result_dir -- the directory path in which the cwl execution results are stored.
