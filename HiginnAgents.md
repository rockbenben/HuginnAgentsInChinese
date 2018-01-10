# Adioso Agent - 机票价格追踪
`Creates events`

Adioso Agent 可以查询两个城市间，在指定时间内的最低飞机票价格。机票价格货币是美元。查询日期`start_date`和`end_date`之间的差异需小于150天。 需要注册[Adioso](http://adioso.com/)，并在`username` and `password`中输入。

# Aftership Agent - 物流追踪-API付费
`Creates events`

Aftership agent 帮助你追踪你的快递，并实时更新包裹动态。为了能够使用Aftership API，您需要生成一个`API Key`。 这需要付费才能使用其跟踪功能。

操作说明：
Provide the `path` for the API endpoint that you’d like to hit. For example, for all active packages, enter `trackings` (see https://www.aftership.com/docs/api/4/trackings), for a specific package, use `trackings/SLUG/TRACKING_NUMBER` and replace `SLUG` with a courier code and `TRACKING_NUMBER` with the tracking number. You can request last checkpoint of a package by providing `last_checkpoint/SLUG/TRACKING_NUMBER` instead.

You can get a list of courier information here `https://www.aftership.com/courier`

Required Options:

*   `api_key` - YOUR_API_KEY.
*   `path request and its full path`

# Algorithmia Agent - AI算法
`Creates events` `Receives events` `Dry runs` 
[huginn_algorithmia_agent](http://huginnio.herokuapp.com/agent_gems#huginn_algorithmia_agent)

AlgoritmiaAgent运行Algorithmia中的算法。在使用此代理之前，您需要注册一个[Algorithmia](https://algorithmia.com)帐户。

The created event will have the output from Algorithmia in the `result` key. To merge incoming Events with the result, use `merge` for the `mode` key.

# Attribute Difference Agent - 属性差异（待深入理解）
`Creates events` `Receives events`

The Attribute Difference Agent receives events and emits a new event with the difference or change of a specific attribute in comparison to the previous event received.
Attribute Difference Agent 用于传递两个不同值的差异和改变。

`path` specifies the JSON path of the attribute to be used from the event.

`output` specifies the new attribute name that will be created on the original payload and it will contain the difference or change.

`method` specifies if it should be…

*   `percentage_change` eg. Previous value was `160`, new value is `116`. Percentage change is `-27.5`
*   `decimal_difference` eg. Previous value was `5.5`, new value is `15.2`. Difference is `9.7`
*   `integer_difference` eg. Previous value was `50`, new value is `40`. Difference is `-10`

`decimal_precision` defaults to `3`, but you can override this if you want.

`expected_update_period_in_days` is used to determine if the Agent is working.

The resulting event will be a copy of the received event with the difference or change added as an extra attribute. If you use the `percentage_change` the attribute will be formatted as such `{{attribute}}_change`, otherwise it will be `{{attribute}}_diff`.

All configuration options will be liquid interpolated based on the incoming event.

# ~~Basecamp Agent - Service停用~~
`Creates events` `37signals`

The Basecamp Agent checks a Basecamp project for new Events

To be able to use this Agent you need to authenticate with 37signals in the [Services](http://huginnio.herokuapp.com/services) section first.

# Bigquery Agent - 大型数据库分析
`Creates events` `Receives events` `Dry runs`
[huginn_bigquery_agent](http://huginnio.herokuapp.com/agent_gems#huginn_bigquery_agent)

Bigquery Agent 会调用 Google BigQuery 和 Goolge Cloud 账户，可能需要付费。同时，Bigquery Agent 依靠服务帐户进行身份验证，而不是oauth。

Setup:
1.  Visit [the google api console](https://code.google.com/apis/console/b/0/)
2.  Use your existing project (or create a new one)
3.  APIs & Auth -> Enable BigQuery
4.  Credentials -> Create new Client ID -> Service Account
5.  Download the JSON keyfile and either save it to a path, ie: `/home/huginn/Huginn-5d12345678cd.json`, or copy the value of `private_key` from the file.
6.  Grant that service account access to the BigQuery datasets and tables you want to query.

The JSON keyfile you downloaded earlier should look like this:

```
{
  "type": "service_account",
  "project_id": "huginn-123123",
  "private_key_id": "6d6b476fc6ccdb31e0f171991e5528bb396ffbe4",
  "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
  "client_email": "huginn@huginn-123123.iam.gserviceaccount.com",
  "client_id": "123123...123123",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/huginn%40huginn-123123.iam.gserviceaccount.com"
}
```

Agent Configuration:

`project_id` - The id of the Google Cloud project.

`query` - The BigQuery query to run. [Liquid](https://github.com/cantino/huginn/wiki/Formatting-Events-using-Liquid) formatting is supported to run queries based on receiving events.

`use_legacy` - Whether or not to use BigQuery legacy SQL or standard SQL. (Defaults to `false`)

`max` - Maximum number of rows to return. Defaults to unlimited, but results are always [limited to 10 MB](https://googlecloudplatform.github.io/google-cloud-ruby/#/docs/google-cloud-bigquery/v0.27.0/google/cloud/bigquery/project).

`timeout` - How long to wait for query to complete (in ms). Defaults to `10000`.

`event_per_row` - Whether to create one Event per row returned, or one event with all rows as `results`. Defaults to `false`.

**Authorization**

`keyfile` - (String) The path (relative to where Huginn is running) to the JSON keyfile downloaded in step 5 above.

Alternately, `keyfile` can be a hash:

`keyfile` `private_key` - The private key (value of `private_key` from the downloaded file). [Liquid](https://github.com/cantino/huginn/wiki/Formatting-Events-using-Liquid) formatting is supported if you want to use a Credential. (E.g., `{% credential google-bigquery-key %}`)

`keyfile` `client_email` - The value of `client_email` from the downloaded file. Same as the service account email.

# ~~Boxcar Agent - iPhone 通知栏app(iOS 10以下)~~
`Receives events`

The Boxcar agent sends push notifications to iPhone.

To be able to use the Boxcar end-user API, you need your `Access Token`. The access token is available on general “Settings” screen of Boxcar iOS app or from Boxcar Web Inbox settings page.

Please provide your access token in the `user_credentials` option. If you’d like to use a credential, set the `user_credentials` option to `{% credential CREDENTIAL_NAME %}`.

Options:

*   `user_credentials` - Boxcar access token.
*   `title` - Title of the message.
*   `body` - Body of the message.
*   `source_name` - Name of the source of the message. Set to `Huginn` by default.
*   `icon_url` - URL to the icon.
*   `sound` - Sound to be played for the notification. Set to ‘bird-1’ by default.

# Change Detector Agent - 监测数据变化
`Creates events` `Receives events`

The Change Detector Agent receives a stream of events and emits a new event when a property of the received event changes.
Change Detector Agent 会接收数据流内容，并在监测到数据属性改变后，传递出新时间

`property` specifies a [Liquid](https://github.com/huginn/huginn/wiki/Formatting-Events-using-Liquid) template that expands to the property to be watched, where you can use a variable `last_property` for the last property value. If you want to detect a new lowest price, try this: `{% assign drop = last_property | minus: price %}{% if last_property == blank or drop > 0 %}{{ price | default: last_property }}{% else %}{{ last_property }}{% endif %}`

`expected_update_period_in_days` is used to determine if the Agent is working.

The resulting event will be a copy of the received event.

# Commander Agent - 触发命令
`Receives events` `Controls agents`

Commander Agent 由时间表或传入事件触发，并触发其他agents运行，禁用，配置或启用自己。

**Action types**

Set `action` to one of the action types below:

*   `run`: Target Agents are run when this agent is triggered.

*   `disable`: Target Agents are disabled (if not) when this agent is triggered.

*   `enable`: Target Agents are enabled (if not) when this agent is triggered.

*   `configure`: Target Agents have their options updated with the contents of `configure_options`.

Here’s a tip: you can use [Liquid](https://github.com/huginn/huginn/wiki/Formatting-Events-using-Liquid) templating to dynamically determine the action type. For example:

*   To create a CommanderAgent that receives an event from a WeatherAgent every morning to kick an agent flow that is only useful in a nice weather, try this: `{% if conditions contains 'Sunny' or conditions contains 'Cloudy' %}` `run{% endif %}`

*   Likewise, if you have a scheduled agent flow specially crafted for rainy days, try this: `{% if conditions contains 'Rain' %}enable{% else %}disabled{% endif %}`

*   If you want to update a WeatherAgent based on a UserLocationAgent, you could use `'action': 'configure'` and set ‘configure_options’ to `{ 'location': '{{_location_.latlng}}' }`.

*   In templating, you can use the variable `target` to refer to each target agent, which has the following attributes: `name`, `options`, `sources`, `type`, `url`, `id`, `disabled`, `memory`, `controllers`, `schedule`, `keep_events_for`, `propagate_immediately`, `working`, `receivers`, and `control_targets`.

**Targets**

Select Agents that you want to control from this CommanderAgent.

# Csv Agent 
`Creates events` `Receives events` `Consumes file pointer`

The `CsvAgent` parses or serializes CSV data. When parsing, events can either be emitted for the entire CSV, or one per row.

Set `mode` to `parse` to parse CSV from incoming event, when set to `serialize` the agent serilizes the data of events to CSV.

### Universal options

Specify the `separator` which is used to seperate the fields from each other (default is `,`).

`data_key` sets the key which contains the serialized CSV or parsed CSV data in emitted events.

### Parsing

If `use_fields` is set to a comma seperated string and the CSV file contains field headers the agent will only extract the specified fields.

`output` determines wheather one event per row is emitted or one event that includes all the rows.

Set `with_header` to `true` if first line of the CSV file are field names.

This agent can consume a ‘file pointer’ event from the following agents with no additional configuration: `FtpsiteAgent`, `LocalFileAgent`, `S3Agent`. Read more about the concept in the [wiki](https://github.com/huginn/huginn/wiki/How-Huginn-works-with-files).

When receiving the CSV data in a regular event use [JSONPath](http://goessner.net/articles/JsonPath/) to select the path in `data_path`. `data_path` is only used when the received event does not contain a ‘file pointer’.

### Serializing

If `use_fields` is set to a comma seperated string and the first received event has a object at the specified `data_path` the generated CSV will only include the given fields.

Set `with_header` to `true` to include a field header in the CSV.

Use [JSONPath](http://goessner.net/articles/JsonPath/) in `data_path` to select with part of the received events should be serialized.
