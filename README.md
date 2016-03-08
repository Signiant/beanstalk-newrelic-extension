# beanstalk-newrelic-extension
An AWS Elastic Beanstalk extension to install and configure newrelic for Java tomcat containers

## Beanstalk Variables
* NEWRELIC_JAVA_PLUGIN_URL: The location to download the newrelic agent from (default:         https://oss.sonatype.org/content/repositories/releases/com/newrelic/agent/java/newrelic-java/3.21.0/newrelic-java-3.21.0.zip)
* SERVER_MONITOR_RPM: The location to download the server monitoring agent from (default: https://download.newrelic.com/pub/newrelic/el5/i386/newrelic-repo-5-3.noarch.rpm)
* NEWRELIC_LICENSE_KEY: Your license key to use for this environment
* NEWRELIC_ENABLE: whether to enable or disable the Java agent (use yes or no; default no)
 
## Workings
This beanstalk extension will install and configure the newrelic Java agent along with the server monitoring agent.  What makes it a little unusual is that we need to run a small script called getvars.sh to get the variables from the environment since the newrelic agent must be installed using **commands** but env vars are only available in **container_commands**.  Using this technique allows us to install and configure the agent before the tomcat server is started.

The actual newrelic install script is pretty straightforward - it downloads the agent and the server monitor and configures them.  Note that we always have to install the agent even if we're using it disabled because the JVMOptions parameter references the newrelic jar file and tomcat will not start if this is not present.
