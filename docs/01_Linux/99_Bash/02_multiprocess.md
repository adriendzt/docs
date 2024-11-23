---
title: Multiprocess
layout: default
nav_order: 2
parent: "Bash"
---

We want to create a bash script which can execute several commands in parallel and wait for them to be finished after continuing:


```bash

function long_task(){
  sleep 1;
}

START=$(date +%s.%N)

# Use "&" to run the function in parallel as background tasks
# Not more than THEADS * 100 tasks in background
NB_CPU=`nproc --all`
THREADS=$(( $NB_CPU * 100 ))
echo "Number of THREADS: $THREADS"

for i in $(seq 1 400);
do
  # Every THREADSth job, stop qnd wait for everything to complete
  if (( i % THREADS == 0 )); then
    wait
  fi
  
  long_task &
  
done

# Wait for all background tasks to finish
wait

END=$(date +%s.%N)
DIFF=$(echo "$END - $START" | bc)
echo $DIFF


```

The script should take 400 / NB_CPU x 100 seconds instead of 400 seconds.
