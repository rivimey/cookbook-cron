Description
===========

Installs the cron package and starts the crond service.

Requirements
============

Platforms:

* RHEL family
* Debian family

Provider and Defining Jobs
==========================

You can use the `cron_job` provider or define cron jobs within your chef 
configuration. In your configuration, you can define basic cron jobs or 
point to custom files that define cron jobs.

cron_job provider
-----------------
    
    cron_job name do
        jobinfo params
    end

where `name` is a unique name for the job and `params` is a hash of the following. Time parameters default to "*"

    minute
    hour
    day
    month
    weekday
    command
    
Defining cron jobs in configuration
-----------------------------------

1. For basic cron jobs, you can use the `cron_job` provider multiple times by 
defining node[:cron][:crond] with a hash of name => cron_job hash pairs. 

        "cron": {
            "crond": {
                "echo": {
                    "minute": "5",
                    "command": "echo This is a job that runs every hour at 5 minutes past the hour"
                }
            }
        }


2. Or you can enter the exact lines you would like to appear in `/etc/crontab` 
by defining a list of lines under node[:cron][:crontab_lines].

        "cron": {
            "crontab_lines": [
                "5 * * * * echo This is a job that runs every hour at 5 minutes past the hour"
            ]
        }


3. For custom cron jobs, and to use variables, create a template in 
`templates/<default>/etc/cron.d/#{name}.erb`. Make sure any template 
variables you include in the template are defined in your configuration. 
Enter any such custom jobs you want to run as a list under 
node[:cron][:predefined]. For example, this would render `custom-job.erb` 
and add it to `/etc/cron.d/custom-job` on the host. An example for 
sending email using the django-mailer project is included, but it depends 
on custom variables being defined in your node configuration.

        "cron": {
            "predefined": ["custom-job"]
        }


License and Author
==================

Original Author:: Joshua Timberman (<joshua@opscode.com>)

Copyright 2010-2012, Opscode, Inc.

Provider and Defining Jobs:: David Marble (<davidmarble@gmail.com>)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
