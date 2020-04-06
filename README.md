# mongo

TODO: Enter the cookbook description here.

# Chef and Test Kitchen Lab

## Timings

60 - 90 Minutes

## Summary

We need to create a resuable chef cookbook for installing and managing mongodb servers.

## Tasks

Create a new cookbook called mongo

Create a ChefSpec suite that tests for the following:

* Sources list is updated
* mongodb-org is added to the sources list ( there is a matcher for this )
* MongoDB will be installed

And InSpec tests for the following:

* MongoDB is installed
* MongoDB is version 3.*

Create a recipe that installs and configures this cookbook correctly to pass all these tests.
## Update source code


```ruby
apt_update
```

## Install mongodb package and running

```ruby

package 'mongodb' do
 version '1:3.6.3-0ubuntu1'
end

service 'mongodb' do
  action [:enable, :start]
end
```

## rspec Tests

```ruby
it 'should install mongodb' do
  expect(chef_run).to install_package 'mongodb'
end

it 'should show that mongodb version is 3.0.0' do
  expect(chef_run).to start_service 'mongodb'
end

it 'mongo-org should be in source list' do
  expect{ is_expected.to render_file('/etc/apt/source.list').with_content("mongo")}
end
```

##  kitchen Tests

```ruby
describe service 'mongodb' do
	it { should be_running }
end

describe port(27017) do
  it { should be_listening }
end


describe package 'mongodb' do
  it { should be_installed }
  its("version"){ should match /3\./ }
end
```
