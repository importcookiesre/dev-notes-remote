

# What is Jenkins ?

Jenkins is an open source continuous integration (CI) server. It manages and controls several stages of the software delivery process, including build, documentation, automated testing, packaging, and static code analysis. Jenkins is a highly popular DevOps tool used by thousands of development teams.

Jenkins automation is commonly triggered by code changes in repositories like GitHub, Bitbucket, and GitLab, and integrates with build tools like Maven and Gradle.


# What is CI / CD ?

**Continuous Integration (CI)** is the practice where developers frequently merge their code changes into a central repository. Each of these integrations triggers an automated build and a series of automated tests. This frequent integration helps in identifying and resolving bugs early in the development cycle, preventing the "integration hell" that often occurs when developers work in isolation for long periods and then try to merge their changes all at once.

Following a successful Continuous Integration stage, the process moves to **Continuous Delivery (CD)** or **Continuous Deployment (CD)**. While often used interchangeably, there is a subtle but important distinction between the two.

- **Continuous Delivery:** This practice takes CI a step further by automatically deploying every code change that passes the automated tests to a testing or staging environment. The key principle here is that the codebase is always in a "releasable" state. However, the final push to the production environment—making it available to end-users—is a manual step, often triggered by a business decision.
    
- **Continuous Deployment:** This is the most advanced stage of the CI/CD pipeline. Every change that passes through all the automated tests is automatically deployed to the production environment without any human intervention. This approach allows for a constant stream of new features and bug fixes to be delivered to users.

CI - Build and Test application when ever new commits are pushed into the branch
CD - Continuous Integration + Deploy to production 

# Environment phases

1. Dev environment : Project developement (development team)
2. Q.A / Testing environment : Quality Assurence env and testing (Testing team)
3. UAT environment : User Acceptance Testing
4. Pilot Env or Pre Production environment : Performance Test
5. Live in Production.

