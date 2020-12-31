---
layout: default
title: Smithproxy updates - Dec/31
date: '2020-12-31 18:00:00 +0100'
categories: updates
---

Hi all,
welcome back at [**smithproxy**](https://www.smithproxy.org "Your favorite project's homepage") irregular development update!

# [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage")

## What's new

### Cli development
Most of effort this month has been spent on CLI. CLI was few times partly rewritten, because of couple of hidden bugs
 and "features" of libcli and libconfig. Yeah, some not really rewarding hacking.
But - CLI is now quite well usable: you can add, edit, remove config items, you can move policies around. With this, 
some new mechanics had to be implemented to make work some default values.

For example, if you create a new policy, there are few items set with empty values, some to reasonable defaults, some
 must have existing object references.
For that, I created quite extensive groovy-style `CliHelp` and `CliElement` helper structures, which can describe
  which values are good or can't be accepted:

##### Code snippets
Let's see example of custom lambda `port_check`  filter, which utilizes already existing `CliElement::VALUE_UINT_NZ` filter checking for value convertible to non-zero integer. But this not enough, we have to be checking value range, too.
```C++

struct CliElement {
    
    // ...

    static inline std::function<value_filter_fn> VALUE_UINT_NZ = [](std::string const& v) -> value_filter_retval {
        auto nv = safe_val(v);
        if(nv > 0)
            return std::make_pair( nv, "");
        else
            return std::make_pair(std::any(), "value must be greater than zero");

    };
}

// ...

void CliHelp::init() {
    // ....
    auto port_check = [](std::string const& v) -> CliElement::value_filter_retval {  
    
      auto [ may_val, descr ] = CliElement::VALUE_UINT_NZ(v);  
      auto err = "port value must be a number in range <1024,65535>";  
   
      if(may_val.has_value()) {  
            int port_value = std::any_cast<int>(may_val);  
  
            if(port_value < 1024 or port_value > 65535)  
                   return std::make_pair(std::any(), err);  
            else return { may_val, "" };  
      }  
      return { may_val, err };  
   };  
   add("settings.plaintext_port", "base divert port for non-SSL TCP traffic")  
         .help_quick("<string>: string value of port number")  
         .may_be_empty(false)  
         .value_filter(port_check);
   // ....
```
By the end of the above example, we can see groovy-style API calls  to add features to `settings.plaintext` config value represented in `CliElement` structure.

When value is being written into the running config, following, quite elegant code is called to actually verify value user inputted:
```C++
bool CliHelp::value_check(std::string const& varname, 
                          std::string const& target_value, cli_def* cli) {
  // simplified                            
  auto cli_e = find(varname);
    
  bool may_be_empty = true;  
  bool value_filter_check = true;  
  std::string value_filter_check_response;  
  
  if(cli_e.has_value()) {  
      may_be_empty = cli_e->get().may_be_empty();  
      
      if(not target_value.empty()) {  
         // invoke the filter
         auto[ret, msg] = std::invoke(cli_e->get().value_filter(), target_value);  
         value_filter_check = ret.has_value();  
         value_filter_check_response = msg;  
      }  
  }
  // ...                            
```
Code above gives a freedom of many possible filters and custom values, while provides user with a good feedback what he is supposed to enter as a valid value.

### Certificate Transparency support
Smithproxy now supports CT checks for outbound connections.  You can tweak the option in `tls_profiles`, it's enabled by default:
```
tls_profiles =  
{  
   default =  
   {  
      inspect = true
      # ...
      ct_enable = true     # <<<< 
```
CT checks will however only work if you download public CT keys to actually really verify SCT signatures.
Smithproxy comes with helper command `sx_download_ctlog` which will download ready-made CT keys list. 


###  New build system (again)
Yes, again. The older, gen1 system has been replaced with new scripts, doing the job almost perfect. It utilizes docker on localhost and remotes, to build all supported packages on both amd64 and aarch64 platforms.  It generates checksums, copies Release Notes, etc.
All is then uploaded to file site. 
Currently, I am limited mostly with my www sites limitations. There will be changes in this area quite soon, before 1.0.0 is released.

### Docker inline scenario finally tested
This is quite interesting deployment scenario, when one has everything in the docker: both the client app and smithproxy.  
You can read about it [in this article](https://smithproxy.readthedocs.io/en/latest/scenarios/#inline-docker-scenario) in the smithproxy documentation. Which deserves some more love I must admit.

# Summary
Year 2020 was not really great. Smithproxy made its progress, but not to the extent I would like. Also due my covid infection and quite disappointing recovery, which took more time than for others.  
I am hoping for better year of 2021.  For all of us.

Happy New Year !

Thanks for reading, till next time!
  Ales, [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage") dev
