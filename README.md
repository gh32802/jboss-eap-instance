# Role jboss-eap-instance

## Creates instances of Jboss-EAP Application server

### Dependencies:

- role java

### Configuration

- Config hash "eap_instances"
- Config files from jboss-eap distribution ($JBOSS_HOME_DIR/standalone/configuration/*)
- Inserts for DATASOURCE and MQ configuration parts

Example config hash in vars file "jboss-eap.yml":

```shell
eap_instances:
  - name: 'eap8080'
    enable: False
    ensure: 'present'
    status: 'running'
    is_managed: True
    eap_version: '6.4.4'
    java_version: '8u111'
    jboss_opts: []
    java_opts:
      - '-Xms1024m'
      - '-Xmx1024m'
      - '-XX:MaxMetaspaceSize=256m'
      - '-Djava.net.preferIPv4Stack=true'
      - '-Djboss.modules.system.pkgs=$JBOSS_MODULES_SYSTEM_PKGS'
      - '-Djava.awt.headless=true'
      - '-Djboss.modules.policy-permissions=true'
      - '-Djboss.socket.binding.port-offset=0'
      - '-Djboss.node.name=$NODENAME'
      - '-Djboss.bind.address=0.0.0.0'
      - '-Djboss.bind.address.management=127.0.0.1'
    standalone_xml:
      deploy: True
      src: "{{ wxx_dirs.data_dir }}/deploy/{{ wsa_environment }}/{{ wsa_nodegroup }}/eap8080"
    standalone_inserts:
      - name: 'DATASOURCE'
        file: 'standalone.ds'
      - name: 'MESSAGEQUEUE'
        file: 'standalone.mq'
    config_files: []
    module_base_path: 'modules/system/layers/base'
    apps:
      - path: 'ROOT'
        file: 'ROOT.war'
        src: "{{ resources }}/apps"
        ensure: 'absent'
        remote_src: 'no'
    modules:
      - name: 'ibm/db2'
        version: '4.19.26'
        path: 'de/rewe/ibm/db2'
        ensure: 'present'
        file: 'db2jcc.tar.gz'
      - name: 'oracle/jdbc'
        version: '11.2.0.2.0'
        path: 'de/rewe/oracle/jdbc'
        ensure: 'present'
        file: 'ojdbc6.tar.gz'
      - name: 'lema'
        version: '0.9.14'
        path: 'de/rewe/lema'
        ensure: 'present'
        file: 'lema.tar.gz'
      - name: 'rsm/security'
        version: '1.4RC1'
        path: 'de/rewe/rsm/security'
        ensure: 'present'
        file: 'rsm-security.tar.gz'
      - name: 'bigl/bigl-log-formatter'
        version: '1.0.0'
        
eap_instances:
  - name: eap-vpu
    ensure: present
    enable: True
    status: running
    is_managed: True
    eap_version: 7.1.0
    eap_basedir: /opt
    #java_version: 8u171
    gclog: True
    port_offset: 0
    java_opts:
      - '-Xms512m'
      - '-Xmx512m'
      - '-XX:MaxMetaspaceSize=128m'
      - '-Djava.net.preferIPv4Stack=true'
      - '-Djboss.modules.system.pkgs=$JBOSS_MODULES_SYSTEM_PKGS'
      - '-Djava.awt.headless=true'
      - '-Djboss.modules.policy-permissions=true'
      - '-Djboss.socket.binding.port-offset=$PORTOFFSET'
      - '-Djboss.node.name=$NODENAME'
      - '-Djboss.server.log.dir=$LOGDIR' 
      - '-Djboss.bind.address=0.0.0.0'
      - '-Djboss.bind.address.management=127.0.0.1'
```
