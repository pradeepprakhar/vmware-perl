# vmclone

## Table of Contents

* [Installation](#Installation)
  * [OSX](#OSX)

## Installation

### OSX

Download the vSphere SDK for Perl 6.0 64-bit Linux installer (e.g. VMware-vSphere-Perl-SDK-6.0.0-2503617.x86_64.tar.gz) [here](https://my.vmware.com/group/vmware/get-download?downloadGroup=SDKPERL600) (free registration required) and extract it:
```bash
cd ~/Downloads
tar zxvf VMware-vSphere-Perl-SDK-6.0.0-2503617.x86_64.tar.gz
cd vmware-vsphere-cli-distrib/
```

#### Perl dependencies

* **Alternative 1** - UUID 0.02 + force SDK to use this version (**RECOMMENDED**)

_THS NEEDS TO BE CHECKED IN A BRAND NEW YOSEMITE INSTALL OF OSX_

```bash
sudo cpan Class::MethodMaker Crypt::SSLeay SOAP::Lite UUID LWP XML::LibXML Data::Dumper
```

Patch the Makefile to allow UUID version 0.02
```bash
sudo sed -i '.bak' -e "s/'UUID' => '0.03'/'UUID' => '0.02'/" Makefile.PL
```

* **Alternative 2** - Use CPAN [unauthorized version 0.03](http://search.cpan.org/~cfaber/UUID-0.03/) *BUT* comply with VMware SDK requirements

```bash
curl -s http://www.cpan.org/authors/id/C/CF/CFABER/UUID-0.03.tar.gz -o UUID-0.03.tar.gz && \
tar -xzvf UUID-0.03.tar.gz && \
cd UUID-0.03 && \
perl Makefile.PL && \
make -Wpointer-sign && \
sudo make install \
cd ~/Downloads/vmware-vsphere-cli-distrib
```

```bash
sudo cpan Class::MethodMaker Crypt::SSLeay SOAP::Lite UUID LWP XML::LibXML Data::Dumper
```

#### vSphere SDK for Perl

```bash
sudo perl Makefile.PL
sudo make install
# Copy esxcfg- and vicfg- commands to /usr/local/bin
sudo cp -rv ./bin/ /usr/local/bin/
```

### Ubuntu

#### Perl dependencies

```bash
sudo apt-get -y install libexpat1-dev libxml2-dev libuuid-perl
```

#### vSphere SDK for Perl

Download the vSphere SDK for Perl 6.0 64-bit Linux installer (e.g. VMware-vSphere-Perl-SDK-6.0.0-2503617.x86_64.tar.gz) [here](https://my.vmware.com/group/vmware/get-download?downloadGroup=SDKPERL600) (free registration required) and extract it:

```bash
cd ~/
tar zxvf VMware-vSphere-Perl-SDK-6.0.0-2503617.x86_64.tar.gz
cd vmware-vsphere-cli-distrib/
sudo perl vmware-install.pl
```

Go grab a coffee while CPAN does its thing.

## Testing

_When running from the command line, set the enviroment variable to ignore trusted cert.
Make sure you know what this means!_
```
$ export PERL_LWP_SSL_VERIFY_HOSTNAME=0
```
_Or pass the root CA file path via PERL_LWP_SSL_CA_PATH:_
```
$ export PERL_LWP_SSL_CA_PATH=/path/to/ca/certs
```

List
```bash
vicfg-vmknic --server=$VCENTER__IP --username=$USER@vsphere.local --vihost=$VHOST --list
```

## Usage

```bash
$ perl vmclone2.pl --customize_guest yes \
--username "username@vsphere.local" \
--password $PASSWORD \
--vmhost $VM_HOST \
--vmname $VM_TEMPLATE_NAME \
--vmname_destination $NEW_VM_NAME \
--url https://$VCENTER_IP/sdk/vimService \
--filename ../sampledata/vmclone.xml \
--schema ../schema/vmclone.xsd
```
