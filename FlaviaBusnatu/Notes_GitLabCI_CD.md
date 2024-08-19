# GitLab
- strives to become a complete DevOps platform
- platform on which you build your complete DevOps workflows

# CI/CD
### CI -> Continuous Integration
### CD -> Continuous Deployment or Continuous Delivery  
Automatically and continuously testing, building and releasing code changes to the deployment environment

## Benefits of using GitLab CI/CD
- you already have your code on gitlab so this is an extention of your software development
- seamless integration into code repository
- using CI/CD without overhead of setting it up yourself 
- pipeline configuration as part of your application code
- self-hosted or SaaS(managed)

## GitLab Architecture
1. GitLab Instance or GitLab Server
    - host your application code and pipeline configuration
    - GitLab configurations
    - Manages the pipeline execution 
2. GitLab Runners 
    - Separate machines connected to the GitLab Server machine
    - Agents that run your CI/CD jobs
    - GitLab Server assigns pipeline jobs to available Runners 

GitLab.com is actually a managed gitlab instance that offers multiple managed runners already out of the box  
    