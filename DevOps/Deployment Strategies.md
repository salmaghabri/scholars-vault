# Blue-Green
![](Deployment%20Strategies-3.png)
## pros
Zero downtime during switchovers.
Instant rollback capability in case of issues.
## cons
High resource requirements (double
environments).

Increased complexity in infrastructure
management.
# Canary Deployment
![](Deployment%20Strategies-1.png)
- Allows for detailed performance and user acceptance testing
- Reduced risk of major failures.

- Requires comprehensive monitoring and quick response mechanisms.
- Can be complex to orchestrate for large-scale applications.
# Rolling 
Gradually rolling out changes across different parts of the system.
![](Deployment%20Strategies-5.png)
# Big Bang 
We push all our changes at once
![](Deployment%20Strategies-6.png)
## pros
- Simplicity and straightforward implementation.
- Suitable for small, less complex projects.
## cons
- High risk of bugs and errors.
- Possible system downtime during deployment
# Feature
![](Deployment%20Strategies-2.png)
Introducing new features in the application that can be enabled or disabled without redeploying.
### pros 
High control over feature release to users. 
Ideal for A/B testing and phased feature introduction.
## cons
Potential for increased code complexity. 
Requires diligent management to avoid obsolete toggles.

