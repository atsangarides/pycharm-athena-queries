# athena-queries

Instructions for configuring PyCharm Professional's Database tool to query AWS Athena

## Connecting Pycharm to Athena 
(screenshots included under /setup)
1. Download [Pycharm Professional](https://www.jetbrains.com/pycharm/download/#section=mac) 
(the database tool is not available in the community version)
2. Install the AWS cli; [version 2](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) is the most
up to date at the time of writing these instructions.
3. The authentication config here assumes you have temporary credentials in your `~/.aws/credentials` file
4. In Pycharm, click on the Database button to expand it (should on be on the collapsed right hand side pane)
    - Click on the "+" button
        - Click on "Driver" (This should open a new window with `User Driver` selected in the left sidebar)
            - Name the driver "AWS Athena"
            - Add the driver, selecting `AthenaJDBC42_2.0.25.1001.jar` in the `SimbaAthenaJDBC-2.0.25.1001` folder in this repo
                - After selecting the driver, from the Class dropdown select the `com.simba.athena.jdbc.Driver`
                - Below, add a url template with name `default` and value `jdbc:awsathena://athena.[{host::eu-west-2}].amazonaws.com[:{port::443}][\?<;,{:identifier}={:param}>]`
            - In the Options tab, choose "Redshift" as the SQL dialect
            - Apply -> OK
    - Click on the "+" button
        - Data Source -> "AWS Athena" (the one you just configured)
        - Enter "eu-west-2" in `host` (if not already populated)
        - Click on the Advanced tab and set the following key-value properties:
        ```
            AwsCredentialsProviderArguments = <name of profile in your ~/.aws/credentials file>
            AwsCredentialsProviderClass = com.simba.athena.amazonaws.auth.profile.ProfileCredentialsProvider
            S3OutputLocation = s3://<s3 bucket where Athena outputs query results>
            Workgroup = <your Athena workgroup>
        ```

Some context:
* The `ProfileCredentialsProvider` class we chose will look for AWS credentials in `~/.aws/credentials` on your local 
machine.
* The  `AwsCredentialsProviderArguments` defines which AWS Profile in `~/.aws/credentials` to use.
