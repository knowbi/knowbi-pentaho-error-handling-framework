# Kettle/PDI Error Handling Framework

> Central error handling for your Kettle/PDI projects. Send error codes from anywhere in your Kettle/PDI projects, let the framework handle the error gracefully. 

Error handling in PDI/Kettle is often (if at all) done by copy/pasting the same "Write To Log", "Mail" and other steps or job entries tens of times throughout Kettle/PDI projects. This framework aims to provide a central template project to centralize error handling. 

## Using the Error Handling Framework

The framework is built in Kettle. You'll need 
- Java 8 
- Kettle/PDI: download 8.2 Remix [here](https://s3.amazonaws.com/kettle-neo4j/kettle-neo4j-remix-8.2.0.7-719-REMIX.zip) or the original 8.2. Kettle/PDI client [here](https://sourceforge.net/projects/pentaho/files/Pentaho%208.2/client-tools/pdi-ce-8.2.0.0-342.zip/download).
- clone this repository to a location on your local system.

```sh
git clone https://github.com/knowbi/knowbi-pentaho-error-handling-framework.git
```

## Configuration 

All configuration is done through: 
- a properties file for general system configuration. Copy the template file `pentaho-code-general.properties.template` to `pentaho-code-general.properties`
<details>
<summary>Click for details</summary>

### Database logging configuration
  - `ds.log.db`= (default: ds_log): logging database name
  - `ds.log.host`= (default: localhost): logging database host
  - `ds.log.pass`= (default: none): logging database username 
  - `ds.log.class`= (default: org.postgresql.Driver) : logging database driver 
  - `ds.log.port`= (default: 5432): logging database port 
  - `ds.log.url`= (default: jdbc:postgresql://localhost:5432/ds_log): logging database url 
  - `ds.log.user`= (default: none): database logging username

### Email server configuration 
  - `mail.user.name`= (default: none): SMTP username
  - `mail.user.pass`= (default: none): SMTP password
  - `mail.recv.address`= (default: none): error mail recipient address
  - `mail.recv.name`= (default: none): error mail recipient name
  - `mail.sender.address`= (default: none): error mail sender address
  - `mail.sender.name`= (default: none): error mail sender name 
  - `mail.smtp.port`= (default: none): SMTP server port 
  - `mail.smtp.server`= (default: none): SMTP server host name or IP address

### Log file path    
  - file.path= (default: /tmp): default file path to write log files to 
  
</details>

- an error list in Excel format for easy editing. Copy the template file `error-codes.xlsx.template` to `error-codes.xlsx`. 
<details>
<summary>Click for details</summary>

This template has been populated with a number of example lines. This file should be configured to match your projects and environments before use. 

### Error Codes sheet
This sheet contains the basic error information: error code, descriptions and actions for error handling. 
  - `code`: error code passed in from any job calling this framework. 
  - `message`: error message that goes with the job. Limited use of parameters is supported.  
  - `mail?`: send error mail? (Yes/No)
  - `table?`: write to logging table? (Yes/No)
  - `file?`: write to log file (Yes/No)
  - `abort?`: abort the calling transformation or job? (Yes/No)

### Descriptions
A breakdown of the error code. A typical error code consists of: 
  - `system code`:  the main system (job) the error originates from (e.g. staging, dim, fact load)
  - `subsystem code`: the subsystem (job, transformation) the error originates from (e.g. dim_customers) 
  - `level code`: an error level, e.g. INFO, WARN, ERROR, FATAL, ...   

### Database Info
This sheet contains a breakdown of log/error tables to use per error code. If no table is specified for an error that logs to database, the default tables will be used. 
  - `Default table`: is this the default table for this error code? 
  - `Table`: table name to write error logging information to
  - `Code`: the error code
  - `Column timestamp`: column to log the error timestamp to 
  - `Column message`: column to log the error message to 
  - `Column transformation`: column to log the failing job/transformation to 

### Parameter Error Code 
This sheet defines the parameters to use in the various lower level error handling. 

TODO: evaluate and if necessary refactor/remove 
  - `message value`: parameter message to pass 
  - `parameter`: parameter name
</details>

## Usage 

When an error occurs, determine which error code should be thrown, and pass that error code as a parameter to the framework's main job `jb_handle_errors.kjb` through the `PRM_ERROR_CODE` parameter.

When the framework receives an error code, it will be parsed according to the rules defined in the configuration files. 

## Sample project

A sample project that uses the error handling framework is provided in `sample-project/jb_main_job.kjb`

## Contact, Support

This framework is a template and is not ready for production use without configuration and/or tuning. 

If you do find any issues or have feature requests, please create a [ticket](https://github.com/knowbi/knowbi-pentaho-error-handling-framework/issues).  