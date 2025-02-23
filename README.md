# Understanding compositional evolution of Kubernetes

## Collaborators

| Role      | Name               | Email                      |
| --------- | ------------------ | -------------------------- |
| Mentor    | Shripad J Nadgowda | shripad.nadgowda@intel.com |
| Developer | Jeffrey Chen       | jchen25@bu.edu             |
| Developer | Jinqi Lu (Kevin)   | jinqilu@bu.edu             |
| Developer | Johnson Yang       | johnsony@bu.edu            |
| Developer | Saurabh Singh      | saurabh2@bu.edu            |
| Developer | Usman Jalil        | ujalil@bu.edu              |


## 1. Vision and Goals Of The Project: 

### Background
Kubernetes is incredibly important in the modern cloud as it makes it easier for developers to manage their applications. It provides a framework to manage containerized
applications and services, allowing for deployment and scheduling of containers while upholding scalability. Due to its nature, the software stack of Kubernetes is vastly distributed 
and diverse making it challenging to maintain and update as new releases of stack components become available. 

### Goals
Our primary objective is to develop a method that provides Kubernetes developers with insights into possible vulnerabilities, and other information on both old and new releases of stack components. Key goals for this project include:

- Collecting data about Kubernetes software components 
- Analyzing the chronological evolution of their dependencies
- Building an analytical framework to bring useful insights from this data
- Create a tool for users to view and interact with our analyzed data

## 2. Users/Personas Of The Project:
This project focuses on identifying vulnerabilities and gaps in Kubernetes components, making it relevant to mostly those who plan on developing and maintaining Kubernetes. By knowing which version of each stack component is less vulnerable, developers and maintainers can use those versions to make Kubernetes more secure. By extension, cloud providers generally have APIs that connect to Kubernetes, so a safer Kubernetes would also benefit cloud providers. 

Kubernetes development/maintainers teams
- People who develop Kubernetes would want to see data on their stack components as it lets them know whether they want to upgrade the component to the newest version or the current version is more secure
- People who maintain Kubernetes would like to see the history of these stack components, and they may find a vulnerability in an older version that they might want to patch up 

Cloud providers
- Such as Microsoft Azure, AWS, or Google Cloud

## 3. Scope and Features Of The Project:
- Collecting and analyzing historical data related to Kubernetes’ software components, their dependencies, and how they have evolved. 
- Storing the dataset into a Neo4j graph database
- Designing and developing an analytical framework to extract useful insights from the data

## 4. Solution Concept

![arch_diagram](https://github.com/EC528-Fall-2023/Evolution-of-Kubernetes-/assets/76934261/57b79a18-efa7-41d9-a48d-2bd58d12a4f3)

1. Kubernetes SBOM
- There exists an open-source program that generates a software bill of materials (SBOM) in SPDX form. Using this, we can create and collect the SBOM. This includes listing the components and dependencies that Kubernetes relies on.
- Because the components making up the Kubernetes SBOM may have their own dependencies, and those dependencies may also have their own dependencies and so on, for the scope of this project, we have chosen to go at least 2-3 levels deep.

2. Neo4j
- Gathering historical data on these dependencies and components and modeling it in neo4j.
- We chose to use a graph-type DB as it can help us track the version upgrades efficiently. For example, if versions 1.16 and 1.17 share the same dependencies as each other, they will share common nodes so if a common dependency is upgraded, we do not need to upgrade the dependencies for each of the versions, rather just upgrading one dependency node will upgrade it for all versions. 
- This lets us have a more efficient analytic query, as we only need one instance of each dependency and component. 

3. Analytic Framework
- Build a system to extract valuable insights
- Analyzing historical data and bringing insights about dependencies, vulnerabilities, release frequency, etc.
- Vulnerabilities can be analyzed using the NIST CVE API, this API can be used to retrieve the list of vulneKubernetes from known components.
- Then we can use GitHub API for data like release frequency, number of commits, and the like.
- Assessing the user's current software version and determining whether they should evaluate that version and/or consider upgrading to the next version.
- Some basis for whether the next version is better than the current one includes security, and we can determine this through the use of CVSS (Common Vulnerability Scoring System), and only allowing low or no severity to pass through our recommendation. 

4. CLI
- The user will interact with our data through the use of a CLI.
- Using commands such as “k8s-scan --eval --current=1.17.0” will evaluate the security posture of your current version which in this case is version 1.17.0.
- Or if the user is using 1.16.9 the command “k8s-scan --recommend --current=1.16.9” can recommend the next best version after their current version.
- Furthermore, the CLI should be able to analyze versions until a certain version, in this case, 1.16.0 using the command “k8s-scan --analyze --current=1.16.0 --deps” will do that. 

5. Extra
- Exploring the inclusion of third-party network and storage plugins if time allows, given Kubernetes' extensive ecosystem.


## 5. Acceptance criteria
- A large dataset that's publicly accessible covering the software compositional evolution of Kubernetes. 
- Development of a CLI tool for users to visualize the data, and interact with our data
- Validate our end product with a kubernetes development community, and see if our product resonates with them

## 6. Release Planning:
### Sprint #1 (Sep 20 - Oct 3)
- Learning about the Kubernetes ecosystem, survey, and review the ecosystem.
- Start some architecture (how to collect data, how to store data, which database to use) not necessarily implementing.

### Sprint #2 (Oct 4 - Oct 17)
- Establish the database infrastructure.
- Determine data storage mechanisms
- Create a Proof of Concept (POC)
- Begin designing the system's architecture
- Divide tasks among team members
- Identify necessary APIs

### Sprint #3 (Oct 18 - Oct 31)
- Develop the system 
- Deliver the first MVP
- Ensure data is accessible and queryable

### Sprint #4 (Nov 1 - Nov 14)
- Fine-tune the system.
- Develop a CLI tool and analytics features.
- Analyze the insights gained from the data.

### Sprint #5 (Nov 15 - Nov 28)
- Getting feedback and putting it into our application
