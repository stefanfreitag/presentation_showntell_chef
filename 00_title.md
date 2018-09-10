# Chef



## What the hell is this Chef

![Chef Giphy](https://media.giphy.com/media/10u6gt11vnm812/giphy.gif) <!-- .element: class="fragment" -->

- Configuration management solution <!-- .element: class="fragment" -->
- Used to manage server configuration with automation <!-- .element: class="fragment" -->


## Chef Components

- Cookbook
  - consists of Recipes
- Chef Supermarket
  - contains public cookbooks
- Node
  - has a chef-client installed
- Chef-Server
  - central component
- Chef Developer Kit
  - (Swiss army) knife


![Chef Components](https://docs.chef.io/_images/start_chef.svg)
Source: [https://docs.chef.io/_images/start_chef.svg](https://docs.chef.io/_images/start_chef.svg)


- Roles: Run-list
  - the same for all servers at the same "layer" in the stack
  - different across layers.

- Environments
  - A label that can be assigned to a node

- Data Bags
  - Global source of attributes that any recipe can call upon
  - Data can be encrypted

Note:

- Examples for Roles:  web server, build server
- Examples for Environemnts: Production, Staging, Development
- Data bages e.g. used to store username/ password



## How Chef works

- DevOps engineer writes code that
  - describes the state of a node
  - is deployed to the Chef-server

![Chef Giphy](https://media.giphy.com/media/vo6h46NUgExPy/giphy.gif) <!-- .element: class="fragment" -->


- Chef-server knows the expected state of each server
- Chef-client running on a node
  - queries Chef-server for updates
  - converges the node state



## Examples


### Install a software package

```ruby
package 'tar' do
  action :install
end
```


### Transfer a remote file

```ruby
remote_file '/tmp/testfile' do
  source 'http://www.example.com/tempfiles/testfile'
  mode '0755'
  checksum '3a7dac00b1' # A SHA256 (or portion thereof) of the file.
end
```



## Chef &  Testing

- Writing cookbooks without tests is like

![Chef Giphy](https://media.giphy.com/media/xTiTnwu3tr3netIR0Y/giphy.gif) <!-- .element: class="fragment" -->


- Testing cookbooks on localhost can be challenging
  - Availability of the base image
  - Enterprise malware detection/ anti-virus tools

- Requires virtualization software, e.g. a combination of
  - Vagrant,
  - VMware Workstation or VirtualBox



## Chef & Containers

TODO


## Further information

https://downloads.chef.io/chefdk
https://www.slideshare.net/JoshPadnick/introduction-to-chef-automate-your-infrastructure-by-modeling-it-in-code
