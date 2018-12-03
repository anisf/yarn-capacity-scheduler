yarn-capacity-scheduler
=========

Manage YARN queues via capacity-scheduler

Notice
------------
Sub queues are not supported yet

Requirements
------------

None

Role Variables
--------------

| Variable   | Default | Description  |
| :---       | :---    | :---             |
`yarn_refresh_queues` | `false` | Use "yarn rmadmin -refreshQueues" command or not (bool) |
`yarn_site_path` | `/opt/mapr/hadoop/hadoop-2.7.0/etc/hadoop/hive-site.xml` | "hive-site.xml" configuration file path |
`yarn_site_owner` | `mapr` | "hive-site.xml" configuration file user owner |
`yarn_site_group` | `mapr` | "hive-site.xml" configuration file group owner |
`yarn_site_perms` | `0755` | "hive-site.xml" configuration file permissions |
`capacity_scheduler_path` | `/opt/mapr/hadoop/hadoop-2.7.0/etc/hadoop/capacity-scheduler.xml` | "capacity-scheduler.xml" configuration file path |
`capacity_scheduler_owner` | `mapr` | capacity-scheduler.xml" configuration file user owner |
`capacity_scheduler_group` | `mapr` | capacity-scheduler.xml" configuration file group owner |
`capacity_scheduler_perms` | `0755` | capacity-scheduler.xml" configuration file permissions |
`scheduler_maximum_applications` | `10000` | YARN max number of apps |
`scheduler_maximum_am_resource_percent` | `0.1` | Maximum percent of resources in the cluster which can be used to run application masters |
`scheduler_node_locality_delay` | `40` | Number of missed scheduling opportunities after which the CapacityScheduler attempts to schedule rack-local containers |
`queue_mappings_override_enable` | `false` | Force queue mapping (str) |
`scheduler_root_queue` | [] | Settings for root queue |
`scheduler_queues` | [] | Settings for root children queues |
`scheduler_queue_mapping`| [] | List of queue / User|Group mapping

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:


```
- hosts: resource_managers
  vars:
    capacity_scheduler_path: /etc/hadoop/conf/capacity-scheduler.xml
    capacity_scheduler_owner: yarn
    capacity_scheduler_group: hadoop
    yarn_site_path: /etc/hadoop/conf/yarn-site.xml
    yarn_site_owner: yarn
    yarn_site_group: hadoop
    scheduler_root_queue:
      acl_submit_applications:
        - " "
      acl_administer_queue:
        - hdfs
    scheduler_queues: 
      - name: queue1
        capacity: 30
        max_capacity: 30
        user_limit_factor: 1
        state: RUNNING
        acl_submit_applications:
          - hadoop_user_1
        acl_administer_queue:
          - hdfs

      - name: queue2
        capacity: 20
        max_capacity: 20
        user_limit_factor: 1
        state: RUNNING
        acl_submit_applications:
          - hadoop_user_2
        acl_administer_queue:
          - hdfs

  roles:
    - anisf.yarn-capacity-scheduler
```
License
-------

BSD