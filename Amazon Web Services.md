# AWS

- [AWS](#aws)
  - [S3](#s3)
  - [DynamoDB](#dynamodb)
  - [EC2](#ec2)
  - [Lamda](#lamda)
    - [Lamda Functions](#lamda-functions)
  - [CloudFront](#cloudfront)


[***Aws Flow***](https://www.google.com/search?q=aws+flow&tbm=isch&ved=2ahUKEwiiiPGLiJPpAhVB-KwKHYHaCb8Q2-cCegQIABAA&oq=aws+flow&gs_lcp=CgNpbWcQAzIECCMQJzICCAAyAggAMgIIADICCAAyBggAEAgQHjIGCAAQCBAeMgYIABAIEB4yBggAEAgQHjIGCAAQCBAeOgQIABAYOgQIABAeUKILWKkQYJcRaABwAHgAgAFTiAH-ApIBATWYAQCgAQGqAQtnd3Mtd2l6LWltZw&sclient=img&ei=ZUusXuLBOcHwswWBtaf4Cw&bih=949&biw=1864)

User => Cloudfront CDNs => EC2 <- -> DynamoDB |
                           EC2 => Lamda => S3 => EC2
## S3

- Object Storage Service
- Key: Value Storage
- Upload or Download any file or object you want
- Size Limit: 5 Gb

## DynamoDB

- Fast No-SQL database
- KeyL Value Storage Model

## EC2

- Basic Server - Runs any server you want
- Alternative to DigitalOcean / Heroku
- General Purpose - Bare-metal server

## Lamda

- Run code for any type of application or service
- Provide a function and Lamda will run it
- Scales automatically to fit amount of user needs

### Lamda Functions

  - Takes a little bit of time to start up the function
  - Only runs function when triggered - not charged a fee otherwise

## CloudFront

- Web Server that speeds up distribution of static files
- Works exactly like a CDN
- Provides automatic HTTPS