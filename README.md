# Role to install cassandra 3.11.x

#### Prerequisites
- **Java 1.8** - Can be installed using `java_1_8` role.

#### Settings/Variables

Following variables can be overwritten by passing `extra-vars` option, or in a template created using *ansible tower*

```yaml
---
cluster_name: 'Cassandra cluster'
endpoint_snitch: GossipingPropertyFileSnitch
```

Other configuration can also be specified here. please check `templates` for that.

#### Inventory/hosts
```
[cassandra_hosts]
cassandra1 ansible_host=116.203.142.1 ansible_connection=ssh private_ip=116.203.142.1 public_ip=116.203.142.1 dc=dc1 rack=rack1 seed=true
cassandra2 ansible_host=116.203.142.2 ansible_connection=ssh private_ip=116.203.142.2 public_ip=116.203.142.2 dc=dc1 rack=rack1 seed=true
cassandra3 ansible_host=116.203.142.3 ansible_connection=ssh private_ip=116.203.142.3 public_ip=116.203.142.3 dc=dc1 rack=rack2 seed=false

cassandra4 ansible_host=116.203.142.4 ansible_connection=ssh private_ip=116.203.142.4 public_ip=116.203.142.4 dc=dc2 rack=rack1 seed=true
cassandra5 ansible_host=116.203.142.5 ansible_connection=ssh private_ip=116.203.142.5 public_ip=116.203.142.5 dc=dc2 rack=rack1 seed=true
cassandra6 ansible_host=116.203.142.6 ansible_connection=ssh private_ip=116.203.142.6 public_ip=116.203.142.6 dc=dc2 rack=rack2 seed=false
```

#### Playbook
Sample playbook can be found in `cassandra_3/tests` directory. To install kafka along with all it's dependencies, use
`test.yml` playbook.

#### Note
After installation, please check the service status.

- You can start Cassandra using the service. However, normally the service will start automatically. For this reason be sure to stop it if you need to make any configuration changes.
```
sudo service cassandra start
```

- Verify that Cassandra is running by invoking `nodetool status` from the command line.
- The default location of configuration files is `/etc/cassandra`.
- The default location of log and data directories is `/var/log/cassandra/` and `/var/lib/cassandra`.
- Start-up options (heap size, etc) can be configured in `/etc/default/cassandra`.

#### Run
```
$ cd roles/cassandra_3/tests
$ ansible-playbook -i inventory test.yml
```

To run it in local, use `[local]` hosts group from `inventory` file, and change `hosts` variable in `test.yml` to -
`hosts: local`

Then run the playbook as -
```
ansible-playbook -i inventory --connection=local test.yml -K
```

#### References

- Download & Install
https://cassandra.apache.org/download/

- Single DC setup -
https://docs.datastax.com/en/ddac/doc/datastax_enterprise/production/DDACsingleDCperWorkloadType.html

- Multi DC setup -
https://docs.datastax.com/en/ddac/doc/datastax_enterprise/production/DDACmultiDCperWorkloadType.html