# Good API

## Good API for every User Type



| User | Characteristics |
|------|-----------------|
| API Consumer | • Self-service & Discovery<br>• For skilled developers<br>• To reduce support cost<br>• Better customer experience<br>• Documented request/response<br>• Developer portal with good categorization<br>• Design and mock environment<br>• Avoid duplicate APIs<br>• Good syntax that helps predict behavior (Name, URI, Response code, Good version)<br>• Scope of API (Single purpose)<br>• Should not allow users to misuse |
| Dev Team | • Flexible and easy to extend<br>• Meet stakeholder needs<br>• Easy to test without spending whole lot of time on documentation<br>• Purpose driven<br>• Easy to read and understand<br>• Good test coverage (unit and integration)<br>• API maturity model<br>• Follow patterns to implement logic:<br>&nbsp;&nbsp;- Resource based routing<br>&nbsp;&nbsp;- Content based routing<br>&nbsp;&nbsp;- Pagination<br>&nbsp;&nbsp;- CRUD support<br>&nbsp;&nbsp;- CQRS<br>&nbsp;&nbsp;- Aggregator/scatter-gather<br>&nbsp;&nbsp;- Orchestration<br>&nbsp;&nbsp;- Webhook for notification |
| SRE | • Geo-routing<br>• Throttling<br>• Rate limiting<br>• Availability<br>• Error handling<br>• Fault tolerance<br>• Circuit breaker<br>• Bulk-head<br>• Proper logging for monitoring and debugging<br>• Correlated logging in microservice world |
| Security | • Public vs Private<br>• Firewall<br>• Access control<br>• Authorization<br>• Encryption of PII data |





## Bad vs Well-Managed API

| Aspect | Bad/Unmanaged API | Well-Managed API |
|--------|-------------------|------------------|
| Discoverability | No discoverability - developers cannot find APIs that suit their requirements | Discoverable - easy for developers to find and explore |
| Documentation | No proper documentation - only interface, no context | Comprehensive documentation |
| Ownership | No way to track business owner or how ownership evolves over time | Proper tracking of business owner and ownership evolution |
| Business Model | No elegant business model possible | Good business model enabled |
| Versioning | No version management - new API could break existing consumers | Good API versioning |
| Authentication | No AuthN - anyone can access anonymously | AuthN: Restricted access |
| Authorization | No access control | AuthZ: Who can do what |
| Rate Limiting | No rate limit - can starve server resources | Rate limiting in place |
| Metrics | No tracking or monitoring | Monitoring health for scalability |
| Scalability | Cannot scale properly due to no usage stats | Scales based on usage patterns |
