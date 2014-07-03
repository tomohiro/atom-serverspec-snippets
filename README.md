Serverspec Snippets in Atom
================================================================================

Snippets to help you writing [Serverspec](http://serverspec.org) specs in
[Atom](http://atom.io)


Example
--------------------------------------------------------------------------------

```ruby
# Serverspec
# Resource Types http://serverspec.org/resource_types.html

# http://serverspec.org/resource_types.html#cgroup
describe 'Linux cgroup resource type' do
  # cgroup + [TAB]
  describe cgroup('group1') do
    its('cpuset.cpus') { should eq 1 }
  end
end

# http://serverspec.org/resource_types.html#command
describe 'Command resource type' do
  # command + [TAB]
  describe command('whoami') do
    # return_stdout + [TAB]
    it { should return_stdout 'root' }
  end

  describe command('ls /foo') do
    # return_stderr + [TAB]
    it { should return_stderr 'ls: /foo: No such file or directory' }
  end

  describe command('ls /tmp') do
    # return_exit_status + [TAB]
    it { should return_exit_status '0' }
  end

  # command_stdout + [TAB]
  describe command('ls -al /') do
    its(:stdout) { should match /bin/ }
  end

  # command_stderr + [TAB]
  describe command('ls /foo') do
    its(:stderr) { should match /No such file or directory/ }
  end
end

# http://serverspec.org/resource_types.html#cron
describe 'Cron resource type' do
  # cron + [TAB]
  describe cron do
    it { should have_entry '* * * * * /usr/bin/foo' }
  end

  # cron_with_user + [TAB]
  describe cron do
    it { should have_entry('* * * * * /usr/bin/foo').with_user('Tomohiro') }
  end
end

# http://serverspec.org/resource_types.html#default_gateway
describe 'Default gateway resource type' do
  describe default_gateway do
    its(:ipaddress) { should eq '192.168.10.1' }
    its(:interface) { should eq 'br0' }
  end
end

# http://serverspec.org/resource_types.html#file
describe 'File and directory resource type' do
  # file + [TAB]
  describe file('/etc/passwd') do
    # be_file + [TAB]
    it { should be_file }
  end

  describe file('/var/log/httpd') do
    # be_directory + [TAB]
    it { should be_directory }
  end

  describe file('/var/run/unicorn.sock') do
    # be_socket + [TAB]
    it { should be_socket }
  end

  describe file('/etc/httpd/conf/httpd.conf') do
    # content + [TAB]
    its(:content) { should match /ServerName www.example.com/ }
  end

  describe file('/etc/sudoers') do
    # be_mode + [TAB]
    it { should be_mode 440 }

    # be_owned_by + [TAB]
    it { should be_owned_by 'root' }

    # be_grouped_into + [TAB]
    it { should be_grouped_into 'wheel' }

    # be_readable + [TAB]
    it { should be_readable }

    # be_readable_by + [TAB]
    it { should by_readable.by('root') }

    # be_readable_by_user + [TAB]
    it { should by_readable.by_user('apache') }
  end

  describe file('/etc/sudoers') do
    # be_writable + [TAB]
    it { should be_writable }

    # be_writable + [TAB]
    it { should by_writable.by('root') }

    # be_writable + [TAB]
    it { should by_writable.by_user('apache') }
  end

  describe file('/etc/init.d/httpd') do
    # be_executable + [TAB]
    it { should be_executable }

    # be_executable_by + [TAB]
    it { should by_executable.by('root') }

    # be_executable_by_user + [TAB]
    it { should by_executable.by_user('httpd') }
  end

  describe file('/') do
    # be_mounted + [TAB]
    it { should be_mounted }

    # be_mounted_with_type + [TAB]
    it { should be_mounted.with( :type => 'ext4' ) }

    # be_mounted_with_options + [TAB]
    it { should be_mounted.with( :options => { :rw => true } ) }

    # be_mounted_only_with + [TAB]
    it do
      should be_mounted.only_with(
        :device  => '/dev/mapper/VolGroup-lv_root',
        :type    => 'ext4',
        :options => {
          :rw   => true,
          :mode => 620
        }
      )
    end
  end

  describe file('C:\\Windows\\System32\\wuapi.dll') do
    # be_version + [TAB]
    it { should be_version('7.6.7600.256') }
  end

  describe file('/etc/services') do
    # match_md5checksum + [TAB]
    it { should match_md5checksum '35435ea447c19f0ea5ef971837ab9ced' }

    # match_sha256checksum + [TAB]
    it { should match_sha256checksum 'a861c49e9a76d64d0a756e1c9125ae3aa6b88df3f814a51cecffd3e89cce6210' }
  end
end

# http://serverspec.org/resource_types.html#group
describe 'Group resource type' do
  # group + [TAB]
  describe group('root') do
    # exist + [TAB]
    it { should exist }

    # have_gid + [TAB]
    it { should have_gid 0 }
  end
end

# http://serverspec.org/resource_types.html#host
describe 'Host resource type' do
  describe host('serverspec.org') do
    # be_resolvable + [TAB]
    it { should be_resolvable }

    # be_resolvable_by_hosts + [TAB]
    it { should be_resolvable.by('hosts') }

    # be_resolvable_by_dns + [TAB]
    it { should be_resolvable.by('dns') }

    # be_reachable + [TAB]
    it { should be_reachable }

    # be_reachable_with + [TAB]
    it { should be_reachable.with( :port => 22 ) }
  end

  # host_address + [TAB]
  describe host('example.com') do
    its(:ipaddress) { should eq '1.2.3.4' }
  end
end

# http://serverspec.org/resource_types.html#iis_app_pool
describe 'IIS Application Pool resource type' do
  describe iis_app_pool('Default App Pool') do
    # have_dotnet_version + [TAB]
    it { should have_dotnet_version('2.0') }
  end
end

# http://serverspec.org/resource_types.html#iis_website
describe 'IIS Website resource type' do
  describe iis_website('Default Website') do
    # exists + [TAB]
    it { should exist }

    # be_enabled + [TAB]
    it { should be_enabled }

    # be_running + [TAB]
    it { should be_running }

    # be_in_app_pool + [TAB]
    it { should be_in_app_pool('Default App Pool') }

    # have_physial_path + [TAB]
    it { have_physial_path('C:\\inetpub\\www') }
  end
end

# http://serverspec.org/resource_types.html#interface
describe 'Network interface resource type' do
  # interface + [TAB]
  describe interface('eth0') do
    # have_ipv4_address + [TAB]
    it { should have_ipv4_address('192.168.10.10') }
  end

  # interface_speed + [TAB]
  describe interface('eth0') do
    its(:speed) { should eq 1000 }
  end
end

# http://serverspec.org/resource_types.html#ipfilter
describe 'Ipfilter resource type' do
  # ipfilter + [TAB]
  describe ipfilter do
    it { should have_rule 'pass in quick on lo0 all' }
  end
end

# http://serverspec.org/resource_types.html#ipnat
describe 'Ipnat resource type' do
  # ipnat + [TAB]
  describe ipnat do
    it { should _have_rule 'map net1 192.168.0.0/24 -> 0.0.0.0/32' }
  end
end

# http://serverspec.org/resource_types.html#iptables
describe 'Iptables resource type' do
  # iptables + [TAB]
  describe iptables do
    it { should have_rule '-P ACCCEPT' }

    # have_rule_table_chain + [TAB]
    it { should have_rule('-P INPUT ACCEPT').with_table('mangle').with_chain('INPUT') }
  end
end

# http://serverspec.org/resource_types.html#kernel_module
describe 'Kernel module resource type' do
  # kernel_module + [TAB]
  describe kernel_module('virtio_balloon') do
    it { should be_loaded }
  end
end

# http://serverspec.org/resource_types.html#linux_kernel_parameter
describe 'Linux kernel parameter resource type' do
  # linux_kernel_parameters + [TAB]
  describe 'Linux kernel parameters' do
    context linux_kernel_parameter('net.ipv4.tcp_syncookies') do
      its(:value) { should eq 1 }
    end
  end

  # linux_kernel_parameter + [TAB]
  describe linux_kernel_parameter('kernel.osrelease') do
    its(:value) { should be eq '2.6.32-131.0.15.el6.x86_64' }
  end
end

# http://serverspec.org/resource_types.html#lxc
describe 'LXC(Linux Container) resource type' do
  # lxc + [TAB]
  describe lxc('ct01') do
    # be_running + [TAB]
    it { should be_running }
  end
end

# http://serverspec.org/resource_types.html#mail_alias
describe 'Mail alias resource type' do
  # mail_alias + [TAB]
  describe mail_alias('daemon') do
    it { should be_aliased_to 'root' }
  end
end

# http://serverspec.org/resource_types.html#package
describe 'Package resource type' do
  # package + [TAB]
  describe package('httpd') do
    it { should be_installed }
  end

  # package_installed_by
  describe package('jekyll') do
    it { should be_installed.by('gem').with_version('0.12.1') }
  end
end

# http://serverspec.org/resource_types.html#php_config
describe 'PHP config resource type' do
  # php_config_parameters + [TAB]
  describe 'PHP config prameters' do
    context php_config('default_mimetype') do
      its(:value) { should eq 'text/html'}
    end
  end

  # php_config + [TAB]
  describe php_config('session.cache_expire') do
    its(:value) { should eq '180' }
  end
end

# http://serverspec.org/resource_types.html#port
describe 'Port resource type' do
  # port + [TAB]
  describe port(80) do
    # be_listening + [TAB]
    it { should be_listening }

    # be_listening_with + [TAB]
    it { should be_listening.with('tcp') }
  end
end

# http://serverspec.org/resource_types.html#ppa
describe 'PPA resource type' do
  # ppa + [TAB]
  describe ppa('launchpad-username/ppa-name') do
    # exists + [TAB]
    it { should exist }
  end
end

# http://serverspec.org/resource_types.html#process
describe 'Process resource type' do
  # process + [TAB]
  describe process('memchached') do
    # be_running + [TAB]
    it { should be_running }
  end

  # process_parameter + [TAB]
  describe process('memcached') do
    its(:args) { should match /-c 32000\b/ }
  end
end

# http://serverspec.org/resource_types.html#routing_table
describe 'Routing table resource type' do
  # routing_table + [TAB]
  describe routing_table do
    it do
      should have_entry(
        :destination => '192.168.100.0/24',
        :interface   => 'eth1',
        :gateway     => '192.168.10.1'
      )
    end
  end
end

# http://serverspec.org/resource_types.html#selinux
describe 'SELinux resource type' do
  # selinux + [TAB]
  describe selinux do
    # be_disabled + [TAB]
    it { should be_disabled }

    # be_enforcing + [TAB]
    it { should be_enforcing }

    # be_permissive + [TAB]
    it { should be_permissive }
  end
end

# http://serverspec.org/resource_types.html#service
describe 'Service resource type' do
  # service + [TAB]
  describe service('ntpd') do
    # be_enabled + [TAB]
    it { should be_enabled }

    # be_enabled_with_level + [TAB]
    it { should be_enabled.with_level(3) }

    # be_running + [TAB]
    it { should be_running }

    # be_monitored_by + [TAB]
    it { should be_monitored_by('monit') }
  end

  # be_running_under_supervisor + [TAB]
  describe service('ntpd') do
    it { should be_running.under('supervisor') }
  end

  describe service('DNS Client') do
    # be_installed + [TAB] (Only supported in Windows)
    it { should be_installed }

    # have_start_mode + [TAB] (Only supported in Windows)
    it { should have_start_mode('Manual') }
  end
end

# http://serverspec.org/resource_types.html#user
describe 'User resource type' do
  # user + [TAB]
  describe user('root') do
    # exists + [TAB]
    it { should exist }

    # belong_to_group + [TAB]
    it { should belong_to_group 'root' }

    # have_uid + [TAB]
    it { should have_uid 0 }

    # have_home_directory + [TAB]
    it { should have_home_directory '/root' }

    # have_login_shell
    it { should have_login_shell '/bin/bash' }

    # have_authorized_key
    it { should have_auhtorized_key 'ssh-rsa ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGH foo@bar.local' }
  end
end

# http://serverspec.org/resource_types.html#windows_feature
describe 'Windows Feature resource type' do
  # windows_feature + [TAB]
  describe windows_feature('Minesweeper') do
    it { be_installed }
  end

  describe windows_feature('Web-Webserver') do
    # be_installed_by + [TAB]
    it { should be_installed.by('powershell') }
  end
end

# http://serverspec.org/resource_types.html#windows_registry_key
describe 'Windows Registry Key resource type' do
  # windows_registry_key + [TAB]
  describe windows_registry_key('HKEY_USERS\S-1-5-21\Test MyKey') do
    # exists + [TAB]
    it { should exist }

    # have_property + [TAB]
    it { should have_property('string value') }

    # have_property_type + [TAB]
    it { should have_property('binary value', :type_binary) }

    # have_value + [TAB]
    it { should have_value('test default data') }

    # have_property_value + [TAB]
    it { should have_property_value('binary value', :type_binary, 'dfa0f066') }
  end
end

# http://serverspec.org/resource_types.html#yumrepo
describe 'Yumrepo resource type' do
  # yumrepo + [TAB]
  describe yumrepo('epel') do
    # exists + [TAB]
    it { should exist }

    # be_enabled + [TAB]
    it { should be_enabled }
  end
end

# http://serverspec.org/resource_types.html#zfs
describe 'ZFS resource type' do
  # zfs + [TAB]
  describe zfs('rpool') do
    # exists + [TAB]
    it { should exist }

    # zfs_have_property + [TAB]
    it { should have_property 'mountpoint' => '/rpool', 'compression' => 'off' }
  end
end
```

LICENSE
--------------------------------------------------------------------------------

&copy; 2014 Tomohiro TAIRA.
This project is licensed under the MIT license.
See LICENSE for details.
