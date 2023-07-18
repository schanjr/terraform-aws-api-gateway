<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform Module API-GATEWAY-V2
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module api-gateway-v2 to create new modules using this as baseline
     </p>

<p align="center">

<a href="https://github.com/clouddrove/terraform-aws-api-gateway/releases/latest">
  <img src="https://img.shields.io/github/release/clouddrove/terraform-aws-api-gateway.svg" alt="Latest Release">
</a>
<a href="https://github.com/clouddrove/terraform-aws-api-gateway/actions/workflows/tfsec.yml">
  <img src="https://github.com/clouddrove/terraform-aws-api-gateway/actions/workflows/tfsec.yml/badge.svg" alt="tfsec">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-APACHE-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-api-gateway'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+Module+API-GATEWAY-V2&url=https://github.com/clouddrove/terraform-aws-api-gateway'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+Module+API-GATEWAY-V2&url=https://github.com/clouddrove/terraform-aws-api-gateway'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards strategies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure. 

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies: 

- [Terraform 1.x.x](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)







## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-api-gateway/releases).


Here are examples of how you can use this module in your inventory structure:
### complete Example
```hcl
  module "api-gateway" {
    source        = "clouddrove/api-gateway/aws"
    version       = "1.4.0"

    domain_name                 = "example.cam"
    domain_name_certificate_arn = module.acm.arn
    integration_uri             = module.lambda.arn
    zone_id  = "1234059QJ345674343"
    create_vpc_link_enabled = false
    cors_configuration = {
      allow_credentials = true
      allow_methods     = ["GET", "OPTIONS", "POST"]
      max_age           = 5
    }
    integrations = {
      "ANY /" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        timeout_milliseconds   = 12000
      }
      "GET /some-route-with-authorizer" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        authorizer_key         = "cognito"
      }
      "POST /start-step-function" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        authorizer_key         = "cognito"
      }
    }
  }
```
### vpc_link_api Example
```hcl
  module "api-gateway" {
    source        = "clouddrove/api-gateway/aws"
    version       = "1.4.0"

     name        = "api"
    environment = "test"
    label_order = ["environment", "name"]
    domain_name                  = "example.cam"
    create_vpc_link_enabled = true
    zone_id         = "1`23456059QJZ25345678"
    integration_uri = module.lambda.arn
    domain_name_certificate_arn  = module.acm.arn
    subnet_ids                   = tolist(module.public_subnets.public_subnet_id)
    security_group_ids           = [module.security_group.security_group_ids]
    cors_configuration = {
      allow_credentials = true
      allow_methods     = ["GET", "OPTIONS", "POST"]
      max_age           = 5
    }
    integrations = {
      "ANY /" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        timeout_milliseconds   = 12000
      }
      "GET /some-route-with-authorizer" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        authorizer_key         = "cognito"
      }
      "POST /start-step-function" = {
        lambda_arn             = module.lambda.arn
        payload_format_version = "2.0"
        authorizer_key         = "cognito"
      }
    }
  }
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| access\_log\_settings | Settings for logging access in this stage. | `map(string)` | `{}` | no |
| api\_description | the description of the API. | `string` | `"Manages an Amazon API Gateway Version 2 API."` | no |
| api\_key\_selection\_expression | An API key selection expression. Valid values: $context.authorizer.usageIdentifierKey, $request.header.x-api-key. | `string` | `"$request.header.x-api-key"` | no |
| api\_version | A version identifier for the API | `string` | `null` | no |
| apigatewayv2\_api\_mapping\_enabled | Flag to control the mapping creation. | `bool` | `true` | no |
| attributes | Additional attributes (e.g. `1`). | `list(any)` | `[]` | no |
| authorizer\_type | The authorizer type. Valid values: JWT, REQUEST. For WebSocket APIs, specify REQUEST for a Lambda function using incoming request parameters. For HTTP APIs, specify JWT to use JSON Web Tokens. | `string` | `"JWT"` | no |
| authorizers | Map of API gateway authorizers | `map(any)` | `{}` | no |
| body | An OpenAPI specification that defines the set of routes and integrations to create as part of the HTTP APIs. Supported only for HTTP APIs. | `string` | `null` | no |
| connection\_type | Type of the network connection to the integration endpoint. Valid values: INTERNET, VPC\_LINK. Default is INTERNET. | `string` | `"INTERNET"` | no |
| cors\_configuration | The cross-origin resource sharing (CORS) configuration. Applicable for HTTP APIs. | `any` | `{}` | no |
| create\_api\_domain\_name\_enabled | Flag to control the domain creation. | `bool` | `true` | no |
| create\_api\_gateway\_enabled | Flag to control the api creation. | `bool` | `true` | no |
| create\_default\_stage\_enabled | Flag to control the stage creation. | `bool` | `true` | no |
| create\_routes\_and\_integrations\_enabled | Whether to create routes and integrations resources | `bool` | `true` | no |
| create\_vpc\_link\_enabled | Whether to create VPC links | `bool` | `true` | no |
| credentials\_arn | Part of quick create. Specifies any credentials required for the integration. Applicable for HTTP APIs. | `string` | `null` | no |
| default\_route\_settings | Default route settings for the stage. | `map(string)` | `{}` | no |
| default\_stage\_access\_log\_destination\_arn | ARN of the CloudWatch Logs log group to receive access logs. | `string` | `null` | no |
| default\_stage\_access\_log\_format | Single line format of the access logs of data. Refer to log settings for HTTP or Websocket. | `string` | `null` | no |
| domain\_name | The domain name to use for API gateway | `string` | `null` | no |
| domain\_name\_certificate\_arn | The ARN of an AWS-managed certificate that will be used by the endpoint for the domain name | `string` | `""` | no |
| domain\_name\_ownership\_verification\_certificate\_arn | ARN of the AWS-issued certificate used to validate custom domain ownership (when certificate\_arn is issued via an ACM Private CA or mutual\_tls\_authentication is configured with an ACM-imported certificate.) | `string` | `null` | no |
| enabled | Flag to control the api creation. | `bool` | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `"test"` | no |
| identity\_sources | The identity sources for which authorization is requested. | `list(string)` | <pre>[<br>  "$request.header.Authorization"<br>]</pre> | no |
| integration\_description | Description of the integration. | `string` | `"Lambda example"` | no |
| integration\_method | Integration's HTTP method. Must be specified if integration\_type is not MOCK. | `string` | `"POST"` | no |
| integration\_type | Integration type of an integration. Valid values: AWS (supported only for WebSocket APIs), AWS\_PROXY, HTTP (supported only for WebSocket APIs), HTTP\_PROXY, MOCK (supported only for WebSocket APIs). | `string` | `"AWS_PROXY"` | no |
| integration\_uri | URI of the Lambda function for a Lambda proxy integration, when integration\_type is AWS\_PROXY. For an HTTP integration, specify a fully-qualified URL. | `string` | `""` | no |
| integrations | Map of API gateway routes with integrations | `map(any)` | `{}` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| managedby | ManagedBy, eg 'CloudDrove' | `string` | `"hello@clouddrove.com"` | no |
| mutual\_tls\_authentication | An Amazon S3 URL that specifies the truststore for mutual TLS authentication as well as version, keyed at uri and version | `map(string)` | `{}` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `"api"` | no |
| passthrough\_behavior | Pass-through behavior for incoming requests based on the Content-Type header in the request, and the available mapping templates specified as the request\_templates attribute. | `string` | `"WHEN_NO_MATCH"` | no |
| protocol\_type | The API protocol. Valid values: HTTP, WEBSOCKET | `string` | `"HTTP"` | no |
| repository | Terraform current module repo | `string` | `""` | no |
| route\_key | Part of quick create. Specifies any route key. Applicable for HTTP APIs. | `string` | `null` | no |
| route\_selection\_expression | The route selection expression for the API. | `string` | `"$request.method $request.path"` | no |
| route\_settings | Settings for default route | `map(string)` | `{}` | no |
| security\_group\_ids | A list of security group IDs to associate with. | `list(string)` | `[]` | no |
| subnet\_ids | A list of VPC Subnet IDs to launch in. | `list(string)` | `[]` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map(any)` | `{}` | no |
| target | Part of quick create. Quick create produces an API with an integration, a default catch-all route, and a default stage which is configured to automatically deploy changes. For HTTP integrations, specify a fully qualified URL. For Lambda integrations, specify a function ARN. The type of the integration will be HTTP\_PROXY or AWS\_PROXY, respectively. Applicable for HTTP APIs. | `string` | `null` | no |
| vpc\_links | Map of VPC Links details to create | `map(any)` | `{}` | no |
| zone\_id | The ID of the hosted zone to contain this record. | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| api\_arn | The API identifier. |
| api\_endpoint | The URI of the API, of the form {api-id}.execute-api.{region}.amazonaws.com. |
| api\_id | The API identifier. |
| invoke\_url | URL to invoke the API pointing to the stage |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback 
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-api-gateway/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-api-gateway)!

## About us

At [CloudDrove][website], we offer expert guidance, implementation support and services to help organisations accelerate their journey to the cloud. Our services include docker and container orchestration, cloud migration and adoption, infrastructure automation, application modernisation and remediation, and performance engineering.

<p align="center">We are <b> The Cloud Experts!</b></p>
<hr />
<p align="center">We ❤️  <a href="https://github.com/clouddrove">Open Source</a> and you can check out <a href="https://github.com/clouddrove">our other modules</a> to get help with your new Cloud ideas.</p>

  [website]: https://clouddrove.com
  [github]: https://github.com/clouddrove
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://twitter.com/clouddrove/
  [email]: https://clouddrove.com/contact-us.html
  [terraform_modules]: https://github.com/clouddrove?utf8=%E2%9C%93&q=terraform-&type=&language=
