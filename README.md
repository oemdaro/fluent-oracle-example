# fluent-oracle-example
The Fluentd with Oracle example configuration

## Requirement

- Ruby 2.2.2+ (test with 2.4.1p111)
- [Oracle Instant Client](http://www.rubydoc.info/github/kubo/ruby-oci8/file/docs/install-instant-client.md)
  - Client Package - Basic or Basic Lite
  - Client Package - SDK
  - Client Package - SQL*Plus
- `fluentd` version "~> 0.12.0" 
- `ruby-oci8` version "~> 2.2.5"
- `activerecord-oracle_enhanced-adapter` version "~> 1.8.2"
- `fluent-plugin-sql` version "~> 0.6.1"

The `td-agent` is not working with because it's embedded `ruby 2.1` and `activerecord-oracle_enhanced-adapter` require `ruby 2.2.2`. We need to build ruby from source (or install ruby with dev package).

### RedHat 7 (x86_64)

To build ruby from source just run following command:

```bash
$ yum -y install openssl openssl-devel gcc glibc-devel libgcc gdbm gdbm-devel readline readline-devel libffi libffi-devel zlib zlib-devel
$ wget https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.gz
$ tar -xzvf ruby-2.4.1.tar.gz
$ cd ruby-2.4.1
$ ./configure && make && make install
```

### RedHat 6 (x86_64)

RedHat 6 not working with Oracle Instant client x86_64 which make `ruby-oci8` to crush. So, we need to install 32-bit version for both Ruby and Oracle Instant client.

First allow 32-bit libraries to be installed. From [RH KB 3628](https://access.redhat.com/knowledge/solutions/36238), edit /etc/yum.conf and insert a line in the main section:

```bash
multilib_policy=all
```
and then build ruby using following command.

```bash
$ yum -y install openssl openssl-devel gcc glibc-devel libgcc gdbm gdbm-devel readline readline-devel libffi libffi-devel zlib zlib-devel
$ wget https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.gz
$ tar -xzvf ruby-2.4.1.tar.gz
$ cd ruby-2.4.1
$ CFLAGS="-m32" LDFLAGS="-m32" CXXFLAGS="-m32" ./configure --target=i386-unknown-linux-gnu && make && make install
```

By default, this will install Ruby into /usr/local. To change, pass the `--prefix=DIR` option to the `./configure` script. Use the following commands to check is ruby and gem was installed successfully:

```bash
$ ruby -v
$ gem -v
$ ruby -r mkmf -e ""
```

## Install Fluentd

To install Fluentd and plugin run the following command:

```bash
$ gem install fluentd -v "~> 0.12.0" --no-ri --no-rdoc
$ fluent-gem install ruby-oci8 -v "~> 2.2.5" --no-ri --no-rdoc
$ fluent-gem install activerecord-oracle_enhanced-adapter -v "~> 1.8.2" --no-ri --no-rdoc
$ fluent-gem install fluent-plugin-sql -v "~> 0.6.1" --no-ri --no-rdoc
```
