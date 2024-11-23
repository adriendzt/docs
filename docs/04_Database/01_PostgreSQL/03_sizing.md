---
title: Sizing
layout: default
nav_order: 3
parent: "PostgreSQL"
---

## PostgreSQL settings depending on memory and max connections

Here is some considerations for the PostgreSQL settings depending on the max number of connections and the VM momory available for a Linux server:

VM physical memory - 2GB reserved for the OS > shared_buffers + ((temp_buffers + work_mem) * max_connections) + (maintenance_work_mem * autovaccum_num_workers)
