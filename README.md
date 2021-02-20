# always_free

AWS app framework that uses (almost) only the always-free tier

# Always Free AWS*

Can I write a framework and sample application that uses only the "always free" AWS services?

Also, anyone can create a free app that does nothing, that is nothing new. Can you create a free app that could scale to
a SaaS offering if it needed to?

(*) - Small print. If you outgrow free, these AWS services are not necessarily cheap. AWS charges for bandwidth in ways
that are hard to fathom and the bandwidth is not cheap. Some "must have" services like static web page hosting and a
domain name are not free. Complementary technologies to prevent your free services from being called to many times, e.g.
throttling and caching are not free.

## Key Free Services

- DynamoDB, 25GB
    - But storing 1TB in DynamoDB costs $250 a month or so.
- Cognito, Authentication for your first 50,000 monthly active clients
    - Unless you plan to get paid, you'll probably prefer social media auth.
- Lambda, 1 million calls, ~1 month of compute time
  - At scale you might end up needing to migrate this to ECS or EC2
- SNS/SQS/SES, ~1 million messages each
- Maybe SWF/Step Functions, but free limits are very low.
- CloudWatch/CloudTrail/CloudFormation

[Here is my complete list of the always free services](docs/what_is_free.md).

## Key missing services

### Payment

If you start with pay-per-use services, it sort of implies that you want to monetize this at some point. So you will
need some [payment processor features](https://www.blendr.io/blog/payments-platforms-for-saas/).

### Domain name and Static hosting

I suppose you could ship a desktop client or mobile client, but we want to show off our app to as many people as
possible, so we need at least a place to host static files. We probably also want an HTTPS certificate, and a domain
name. So for a full app you still need to pay for old fashion static webpage hosting.

Cloudfront, S3 and Route 52 costs can run less than $1 a month.

### Networking with private IP addresses (VPC with NAT gateways)

We probably want to put out resources in a VPC and when possibly, move resources to private IP addresses. However!
Managed NAT gateways are not cheap. A NAT gateway is at least $30 a month. Bandwidth is more. Let's plan for success and
assume 1TB of data a month, so $45 for the bandwidth.

### Caching (E.g. Cloudfront, APIGateway, ElasticCache, DAX)

Client side caching is free, but we want to protect our pay per use services from getting called twice unnecessarily.
The various caching options are not free. I'm thinking that the best route is an EC2 or ECS instances with REDIS, especially
if the cache is ephemeral. 

### Throttling (APIGateway)

Throttling with AWS would at least require APIGateway, which is cheap not free.

### Build Pipeline- GitHub's probably better.

AWS offers from free Github-like services, source control, etc. But Gitlab does that too, also for free with more
generous limits.

### Social Media Auth via Google/Facebook/Microsoft is possibly better

Cognito is only free up to certain limits, it might make more sense to use google's authentication.

### Infrastructure as Code

AWS offers some infrastructure-as-code for free (CloudFormation), but you can also do the same with terraform and
terraform.io offers a free plan.

## Making it real

A typical app is a middle tier, data tier and UI.

Today, I'm just adding stubs for the application parts

- `always_free` - python module for things that are not application specific, e.g. boto utils, payment processing, etc.
- `whats_that_modlue` - hypothetical application, name subject to change. Some interesting data source that can respond to
  interesting queries.
- `terraform` - code to spin up dev and production environments in same account

# TODO
Write a framework, add build scripts to bring it into existence. 
