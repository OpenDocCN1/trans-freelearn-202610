# # Examples of installing Windows packages

Using a locally provided file, perform an installation of Ruby 1.9.3-p448. This
package uses the Inno Setup packager and a hint is provided, but remember that
even without it, the provider will try to determine the installer type:

```
```
windows_package "Ruby 1.9.3-p448" do
source File.join("C:", "temp", "rubyinstaller-1.9.3-p448.exe")
options '/dir="C:/Ruby193" /tasks="modpath"'
installer_type :inno
action :install
end
```
```

You can specify arbitrary flags to pass to an installer that does not use one of the
known packaging systems. One example of this is Firefox, which supports passing
the -ms command-line arguments to the installer in order to run in silent mode:

```
```
ff_url = "http://archive.mozilla.org/pub/mozilla.org/mozilla.org/
firefox/releases/28.0b4/win32/en-US/Firefox%20Setup%2028.0b4.exe"
windows_package 'Mozilla Firefox 28.0b4 (x86 en-US)' do
source ff_url
action :install
installer_type :custom
options '-ms'
end
```
```

The options for Firefox can be found at https://wiki.mozilla.org/
Installer:Command_Line_Arguments.

In order to remove a previously installed package, such as iTunes, make use of the
:remove action as follows:

```
```
windows_package "iTunes" do
action :remove
end
```
```

# # Manipulating printers

Chef provides two different resources for managing printers – ports and printers
themselves. The windows_printer_port resource allows you to install TCP/IP
printer ports on a Windows host to connect a printer to. The windows_printer
resource is responsible for an actual printer installation, and they are both used
in conjunction with one another.