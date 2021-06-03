# index

## data-analytics

Processes and utilities for internal data analytics

### Projects

#### etl

Shell scripts for copying data from the main GreenInvoice MySql cluster to analytics dedicated db instances.

#### aggregate-data

Node.js project for aggregating daily/monthly data to the WIDE\_GLOBAL\_DAILY/WIDE\_GLOBAL\_MONTHLY table.

#### send-data

Node.js project for sending a daily/monthly email based on data from the WIDE\_GLOBAL\_DAILY/WIDE\_GLOBAL\_MONTHLY table.

#### zendesk

Export all tickets to S3

### Configuration

All projects require configuration from environment variables:

```bash
export LIVE_HOST=grinvdarr.greeninvoice.internal
export LIVE_USER=aggregationapp
export LIVE_PASSWORD=
export LIVE_SCHEMA=greeninvoice
export DW_HOST=dbrds8.greeninvoice.internal
export DW_USER=aggregationapp
export DW_PASSWORD=
export DW_SCHEMA=greeninvoice
export NOTIF_HOST=dbrds8.greeninvoice.internal
export NOTIF_USER=notificationapp
export NOTIF_PASSWORD=
export NOTIF_SCHEMA=greeninvoice
export AWS_REGION=eu-west-1
export ATHENA_DB=prod
export TMP_S3=s3://greeninvoice-events/query-results
export POSTMARK_TOKEN=
export GOOGLE_CLIENT_EMAIL=daily-data-aggregator@analytics-daily-aggregates.iam.gserviceaccount.com
export GOOGLE_PRIVATE_KEY=

export POSTMARK_TOKEN=

export S3_BUCKET=greeninvoice-events
export S3_EXPORT_FOLDER=transactional/transactional-export
export S3_EXPORT_DB_PREFIX=greeninvoice/greeninvoice.
export S3_PARTITIONED_PREFIX=transactional/transactional-partitioned
export ATHENA_RAW_MYSQL_DB=raw_mysql.
export EXPORT_ROLE_ARN=
export KMS_KEY_ID=
export RDS_DB_INSTANCE=greeninvoice-rds

export ZENDESK_USER=someone@greeninvoice.co.il/token
export ZENDESK_PASSWORD=
export ZENDESK_BUCKET=greeninvoice-events
export ZENDESK_PREFIX=zendesk
```

In addition, mysql and mysqldump bin should be accessible in the PATH

## Development Workflow

#### Write functions

* Use `serverless deploy` only when you've made changes to `serverless.yml` and in CI/CD systems.
* Use `serverless deploy function -f myFunction` to rapidly deploy changes when you are working on a specific AWS Lambda Function.
* Use `serverless invoke -f myFunction -l` to test your AWS Lambda Functions on AWS.
* Open up a separate tab in your console and stream logs in

  ```text
    there via `serverless logs -f myFunction -t`.
  ```

* Write tests to run locally.

**Using stages**

At the very least, use a dev and production stage. Use different AWS accounts for stages. In larger teams, each member should use a separate AWS account and their own stage for development.

**Larger Projects**

Break your application/project into multiple Serverless Services. Model your Serverless Services around Data Models or Workflows. Keep the Functions and Resources in your Serverless Services to a minimum.

**Cheat Sheet**

A handy list of commands to use when developing with the Serverless Framework.

#### Create A Service:

Creates a new Service

`serverless create -p [SERVICE NAME] -t aws-nodejs`

#### Install A Service

This is a convenience method to install a pre-made Serverless Service locally by downloading the Github repo and unzipping it.

`serverless install -u [GITHUB URL OF SERVICE]`

#### Deploy All

Use this when you have made changes to your Functions, Events or Resources in serverless.yml or you simply want to deploy all changes within your Service at the same time.

`serverless deploy -s [STAGE NAME] -r [REGION NAME] -v`

#### Deploy Function

Use this to quickly overwrite your AWS Lambda code on AWS, allowing you to develop faster.

`serverless deploy function -f [FUNCTION NAME] -s [STAGE NAME] -r [REGION NAME]`

#### Invoke Function

Invokes an AWS Lambda Function on AWS and returns logs.

`serverless invoke -f [FUNCTION NAME] -s [STAGE NAME] -r [REGION NAME] -l`

#### Streaming Logs

Open up a separate tab in your console and stream all logs for a specific Function using this command.

`serverless logs -f [FUNCTION NAME] -s [STAGE NAME] -r [REGION NAME]`

