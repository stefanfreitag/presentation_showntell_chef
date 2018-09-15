<!-- markdownlint-disable MD012 MD014 -->

# Chef



## What is this Chef

![Chef Giphy](https://media.giphy.com/media/10u6gt11vnm812/giphy.gif) <!-- .element: class="fragment" -->

- Configuration management solution <!-- .element: class="fragment" -->
- Used to manage server configuration with automation <!-- .element: class="fragment" -->


## Chef Components

- [Chef Supermarket](https://supermarket.chef.io/)
  - contains public cookbooks
- Cookbook
  - consists of Recipes

---

- Chef Server
  - central component
- Node
  - has a chef-client installed

---

- [Chef Development Kit](https://downloads.chef.io/chefdk)
  - e.g. knife, [Test Kitchen](https://github.com/test-kitchen/test-kitchen), [Food Critic](http://www.foodcritic.io/)


![Chef Components](https://docs.chef.io/_images/start_chef.svg)
Source: [https://docs.chef.io](https://docs.chef.io/_images/start_chef.svg)


- Environments
  - Label that can be assigned to a node
- Data Bags
  - Global source of attributes that any recipe can call upon
  - Data can be encrypted
- Roles
  - e.g. web server, build agent

Note:

- Examples for Environemnts: Production, Staging, Development
- Data bages e.g. used to store username/ password



## How Chef works

- DevOps engineer writes code that
  - describes the state of a node
  - is deployed to the Chef server

![Chef Giphy](https://media.giphy.com/media/vo6h46NUgExPy/giphy.gif) <!-- .element: class="fragment" -->


- Chef server knows the expected state of each node
- Chef-client running on a node
  - queries Chef server for updates
  - converges the node state



## Examples


### Generate a cookbook

```shell
$ chef generate cookbook

Usage: chef generate cookbook NAME [options]
    -b, --berks                      Generate cookbooks with berkshelf integration
    -C, --copyright COPYRIGHT        Name of the copyright holder - defaults to 'The Authors'
    -d, --delivery                   This option has no effect and exists only for compatibility with past releases
    -m, --email EMAIL                Email address of the author - defaults to 'you@example.com'
    -a, --generator-arg KEY=VALUE    Use to set arbitrary attribute KEY to VALUE in the code_generator cookbook
    -h, --help                       Show this message
    -I, --license LICENSE            all_rights, apachev2, mit, gplv2, gplv3 - defaults to all_rights
        --pipeline PIPELINE          Use PIPELINE to set target branch to something other than master for the build_cookbook
    -P, --policy                     Use policyfiles instead of Berkshelf
    -V, --verbose                    Show detailed output from the generator
    -v, --version                    Show chef version
    -g GENERATOR_COOKBOOK_PATH,      Use GENERATOR_COOKBOOK_PATH for the code_generator cookbook
        --generator-cookbook
```


### Install a software package

>A package resource block manages a package on a node, typically by installing it.

Source: [https://docs.chef.io](https://docs.chef.io/resource_package.html)

```ruby
package 'tar' do
  action :install
end
```

- No operating system was specified<!-- .element: class="fragment" -->
- How does Chef figure  what to do?<!-- .element: class="fragment" -->
- Ohai provides the required information<!-- .element: class="fragment" -->


```ruby
package 'Install Apache' do
  case node[:platform] # Ohai fills this
  when 'redhat', 'centos'
    package_name 'httpd'
  when 'ubuntu', 'debian'
    package_name 'apache2'
  end
end
```


Some of the supported package types

| Name      | Name         |
| ------------- |-------------|
| apt_package      | cab_package |
| chocolatey_package      | dpkg_package |
| gem_package | homebrew_package    |
| pacman_package |   portage_package |
| rpm_package | solaris_package |
| windows_package | yum_package|
| zypper_package | |


### Transfer a remote file

```ruby
remote_file '/tmp/testfile' do
  source 'http://www.example.com/tempfiles/testfile'
  mode '0755'
  checksum '3a7dac00b1' # A SHA256 (or portion thereof) of the file.
end
```

Source: [https://docs.chef.io](https://docs.chef.io/resource_remote_file.html)



## Chef &  Testing

- Writing cookbooks without tests is like

![Chef Giphy](https://media.giphy.com/media/xTiTnwu3tr3netIR0Y/giphy.gif) <!-- .element: class="fragment" -->


- Requires virtualization software, e.g. a combination of
  - Vagrant,
  - VMware Workstation or VirtualBox
- Testing cookbooks on localhost can be challenging
  - Availability of the base image
  - Enterprise malware detection/ anti-virus tools


### Kitchen

- Provides test harness to execute infrastructure code on one or more platforms in isolation
- Used by all Chef-managed community cookbooks
- ChefDK creates the initial __kitchen.yml__ file automatically
  - It's expected that you will read and edit this file.


#### Example kitchen.yml

```yml
driver:
  name: vagrant
  
provisioner:
  name: chef_zero

verifier:
  name: inspec

platforms:
  - name: centos-6.9

suites:
  - name: default
    run_list:
      - recipe[prometheus::default]
    verifier:
      inspec_tests:
        - test/integration/default
    attributes:
```


#### Commands

```shell
$ kitchen create

$ kitchen converge

$ kitchen setup

$ kitchen verify

$ kitchen destroy
```


#### Tests

##### Users and groups

```shell
describe user('prometheus') do
  it { should exist }
  its('shell') { should eq '/bin/false' }
  its('group') { should eq 'prometheus' }
end

describe group('prometheus') do
  it { should exist }
end
```

##### Files and directories

```shell
describe directory('/opt/prometheus') do
  its('link_path') { should eq '/opt/prometheus-2.3.2.linux-amd64' }
  its('group') { should eq 'prometheus' }
  its('owner') { should eq 'prometheus' }
end

describe directory('/var/log/prometheus') do
  its('group') { should eq 'prometheus' }
  its('owner') { should eq 'prometheus' }
end
```


### kitchen verify

```shell
Target:  ssh://vagrant@127.0.0.1:2222

  User prometheus
     ✔  should exist
     ✔  shell should eq "/bin/false"
     ✔  group should eq "prometheus"
  Group prometheus
     ✔  should exist
  Directory /opt/prometheus
     ✔  link_path should eq "/opt/prometheus-2.3.2.linux-amd64"
     ✔  group should eq "prometheus"
     ✔  owner should eq "prometheus"
  Directory /var/log/prometheus
     ✔  group should eq "prometheus"
     ✔  owner should eq "prometheus"
  
Test Summary: 9 successful, 0 failures, 0 skipped
```



## Further information

- [Kitchen.ci](https://kitchen.ci/)
- [LearnChef.io](https://learn.chef.io/)



![Chef Giphy](https://media.giphy.com/media/acJ6UbGLUB2De/giphy.gif)