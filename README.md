# OpenResty under Rails

If [Rails](http://rubyonrails.org/) designed to maximize the development speed, the [OpenResty](https://openresty.org/) based on [nginx](http://nginx.org/) must being made for maximize the running time speed.

Using the openresty-under-rails gems enable you having the best things provided by OpenResty and still keep your favior Rails stack.

## Installation

### Passenger

You need compile your version of passenger from source code instead of using official one, here is the steps:

```bash
apt-get remove passenger # if you already install passenger
wget https://openresty.org/download/openresty-1.9.7.4.tar.gz
tar xvfz openresty-1.9.7.4.tar.gz
gem install passenger
passenger-install-nginx-module
# Choose 2,No: I want to customize my Nginx installation. (for advanced users)
# Give nginx source code location: /root/openresty-1.9.7.4/bundle/nginx-1.9.7
# Specify a prefix directory: /opt
# Confirm will install an entirely new Nginx installation.
# Record generated script, like: sh ./configure --prefix='/opt' --with-http_ssl_module --with-http_gzip_static_module --with-http_stub_status_module --with-cc-opt=-Wno-error --with-ld-opt='' --add-module='/usr/local/rvm/gems/ruby-2.2.4/gems/passenger-5.0.27/src/nginx_module'
# Confirm yes by enter
cd openresty-1.9.7.4
# Or add more options following https://openresty.org/#Installation
./configure --prefix=/opt \
            --with-pcre-jit \
            --with-ipv6 \
            --without-http_redis2_module \
            --with-http_ssl_module \
            --with-http_gzip_static_module \
            --with-http_stub_status_module \
            --add-module='/usr/local/rvm/gems/ruby-2.2.4/gems/passenger-5.0.27/src/nginx_module'
make
make install
cd ..
rm -rf openresty-1.9.7.4
cd /usr/local/rvm/gems/ruby-2.2.4/gems/passenger-5.0.27/
find . -name "*.o" -type f -delete
vi /opt/nginx/conf/nginx.conf # add pid /run/nginx.pid;
cd /etc/init.d
wget https://gist.githubusercontent.com/Eric-Guo/ae0a63b22e2be1624836/raw/e46ef29ebdb77e15113dfec5919ca823e6b740da/nginx
chmod 755 nginx
update-rc.d -f nginx remove
update-rc.d -f nginx defaults
```