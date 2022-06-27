# CloudFront
Content Delivry Network (CDN). Creates cached copies of your website at various edge locations arround the world.

**Origin:** S3 bucket, EC2 instnace, ELB or Route53
**Edge locations:** server nearby to the users in a AWS region
**Distribution:** collection of Edge locations which defines how cached content should behave

**Two types:** Web and RTMP (streaming media)
**Behaviours:** Redirect to HTTPs, Restrict HTTP Methods, Restrict Viewer Access, Set TTLs
**Invalidations:** invalidates cache on specific files
**Error pages:** serve custom error pages eg. 404
**Restrictions:** Geo restriction to blacklist or whitelist specific countries

- Edge location aren't just read-only, you can write to them eg. PUT objects
- Refreshing cache costs money
- Origin Identity Access (OAI) is used to access private s3 buckets
- Access cached content can be protected via **Signed Urls** or **Signed Cookies**

### !!! I should play a little bit with CloudFront !!!

 ### Lambda@Edge 
Override the behaviour of request and responses:
	
- Viewer request: When CloudFront receives a request from a viewer
- Origin request: Before CloudFront forwards the request to the origin
- Origin resposne: When CloudFront receives a response from the origin
-  Viewer resposne: Before CloudFront returns the response to the viewer

**Use cases:**

- Authenticate requests using Icongito
- Run A/B Testing for marketing startegies