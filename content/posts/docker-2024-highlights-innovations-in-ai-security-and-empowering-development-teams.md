---
title: "Docker 2024 Highlights: Innovations in AI, Security, and Empowering Development Teams"
date: 2025-01-02
categories: 
  - "general"
---

In 2024, as developers and engineering teams focused on delivering high-quality, secure software faster, Docker continued to evolve with impactful updates and a streamlined user experience. This commitment to empowering developers was recognized in the annual Stack Overflow Developer Survey, where Docker ranked as one of the most loved and widely used tools for yet another year. Here’s a look back at Docker’s 2024 milestones and how we helped teams build, test, and deploy with greater ease, security, and control than ever.

![2400x1260 docker evergreen logo blog D 1](https://www.docker.com/wp-content/uploads/2024/12/2400x1260_docker-evergreen_logo_blog_D-1-1110x583.png "- 2400x1260 docker evergreen logo blog D 1")

## Streamlining the developer experience

Docker focused heavily on streamlining workflows, creating efficiencies, and reducing the complexities often associated with managing multiple tools. One big announcement in 2024 is our upgraded Docker plans. With the launch of updated Docker subscriptions, developers now have access to the entire suite of Docker products under their existing subscription. 

The all-in-one subscription model enables seamless integration of Docker Desktop, Docker Hub, Docker Build Cloud, Docker Scout, and Testcontainers Cloud, giving developers everything they need to build efficiently. By providing easy access to the suite of products and flexibility to scale, Docker allows developers to focus on what matters most — building and innovating without unnecessary distractions.

For more details on Docker’s all-in-one subscription approach, check out our Docker plans announcement.

### Build up to 39x faster with Docker Build Cloud

Docker Build Cloud, introduced in 2024, brings the best of two worlds — local development and the cloud to developers and engineering teams worldwide. It offloads resource-intensive build processes to the cloud, ensuring faster, more consistent builds while freeing up local machines for other tasks.

A standout feature is shared build caches, which dramatically improve efficiency for engineering teams working on large-scale projects. Shared caches allow teams to avoid redundant rebuilds by reusing intermediate layers of images across builds, accelerating iteration cycles and reducing resource consumption. This approach is especially valuable for collaborative teams working on shared codebases, as it minimizes duplicated effort and enhances productivity.

Docker Build Cloud also offers native support for multi-architecture builds, eliminating the need for setting up and maintaining multiple native builders. This support removes the challenges associated with emulation, further improving build efficiency.

We’ve designed Docker Build Cloud to be easy to set up wherever you run your builds, without requiring a massive lift-and-shift effort. Docker Build Cloud also works well with Docker Compose, GitHub Actions, and other CI solutions. This means you can seamlessly incorporate Docker Build Cloud into your existing development tools and services and immediately start reaping the benefits of enhanced speed and efficiency.

Check out our build time savings calculator to estimate your potential savings in hours and dollars. 

### Optimizing development workflows with performance enhancements

In 2024, Docker Desktop introduced a series of enterprise-grade performance enhancements designed to streamline development workflows at scale. These updates cater to the unique needs of development teams operating in diverse, high-performance environments.

One notable feature is the Virtual Machine Manager (VMM) in Docker Desktop for Mac, which provides a robust alternative to the Apple Virtualization Framework. Available since Docker Desktop 4.35, VMM significantly boosts performance for native Arm-based images, delivering faster and more efficient workflows for M1 and M2 Mac users. For development teams relying on Apple’s latest hardware, this enhancement translates into reduced build times and a smoother experience when working with containerized applications.

Additionally, Docker Desktop expanded its platform support to include Red Hat Enterprise Linux (RHEL) and Windows on Arm architectures, enabling organizations to maintain a consistent Docker Desktop experience across a wide array of operating systems. This flexibility ensures that development teams can optimize their workflows regardless of the underlying platform, leveraging platform-specific optimizations while maintaining uniformity in their tooling.

These advancements reflect Docker’s unwavering commitment to speed, reliability, and cross-platform support, ensuring that development teams can scale their operations without bottlenecks. By minimizing downtime and enhancing performance, Docker Desktop empowers developers to focus on innovation, improving productivity across even the most demanding enterprise environments.

### More options to improve file operations for large projects

We enhanced Docker Desktop with synchronized file shares (Figure 1), a feature that can significantly improve file operation speeds by 2-10x. This enhancement brings fast and flexible host-to-VM file sharing, offering a performance boost for developers dealing with extensive codebases.

Synchronized file sharing is ideal for developers who:

- Develop on projects that consist of a significant number of files (such as PHP or Node projects).
- Develop using large repositories or monorepos with more than 100,000 files, totaling significant storage.
- Utilize virtual file systems (such as VirtioFS, gRPC FUSE, or osxfs) and face scalability issues with their workflows.
- Encounter performance limitations and want a seamless file-sharing solution without worrying about ownership conflicts.

This integration streamlines workflows, allowing developers to focus more on coding and less on managing file synchronization issues and slow file read times. 

<figure>

![Screenshot of Docker Desktop showing Synchronized file shares within Resources.](https://www.docker.com/wp-content/uploads/2024/12/F1-file-shares-1110x551.png "Screenshot of Docker Desktop showing Synchronized file shares within Resources. - F1 file shares")

<figcaption>

**Figure 1:** Synchronized file shares.

</figcaption>

</figure>

### Enhancing developer productivity with Docker Debug 

Docker Debug enhances the ability of developer teams to debug _any_ container, especially those without a shell (that is, _distroless_ or _scratch_ images). The ability to peek into “secure” images significantly improves the debugging experience for both local and remote containerized applications. 

Docker Debug does this by attaching a dedicated debugging toolkit to any image and allows developers to easily install additional tools for quick issue identification and resolution. Docker Debug not only streamlines debugging for both running and stopped containers but also is accessible directly from both the Docker Desktop CLI and GUI (Figure 2). 

<figure>

![Screenshot of Docker Desktop showing Docker Debug.](https://www.docker.com/wp-content/uploads/2024/12/F2-Docker-Debug.png "Screenshot of Docker Desktop showing Docker Debug. - F2 Docker Debug")

<figcaption>

**Figure 2:** Docker Debug.

</figcaption>

</figure>

Being able to troubleshoot images without modifying them is crucial for maintaining the security and performance of containerized applications, especially those images that traditionally have been hard to debug. Docker Debug offers:

- **Streamlined debugging process:** Easily debug local and remote containerized applications, even those not running, directly from Docker Desktop.
- **Cross-device and cloud compatibility:** Initiate debugging effortlessly from any device, whether local or in the cloud, enhancing flexibility and productivity.

Docker Debug improves productivity and seamless integration. The `docker debug` command simplifies attaching a shell to any container or image. This capability reduces the cognitive load on developers, allowing them to focus on solving problems rather than configuring their environment. 

### Ensuring reliable image builds with Docker Build checks

Docker Desktop 4.33 was a big release because, in addition to including the GA release of Docker Debug, it included the GA release of Docker Build checks, a new feature that ensures smoother and more reliable image builds. Build checks automatically validate common issues in your Dockerfiles before the build process begins, catching errors like invalid syntax, unsupported instructions, or missing dependencies. By surfacing these issues upfront, Docker Build checks help developers save time and avoid costly build failures.

You can access Docker Build checks in the CLI and in the Docker Desktop Builds view. The feature also works seamlessly with Docker Build Cloud, both locally and through CI. Whether you’re optimizing your Dockerfiles or troubleshooting build errors, Docker Build checks let you create efficient, high-quality container images with confidence — streamlining your development workflow from start to finish.

### Onboarding and learning resources for developer success  

To further reduce friction, Docker revamped its learning resources and integrated new tools to enhance developer onboarding. By adding beginner-friendly tutorials, Docker’s learning center makes it easier for developers to ramp up and quickly learn to use Docker tools, helping them spend more time coding and less time troubleshooting. 

As Docker continues to rank as a top developer tool globally, we’re dedicated to empowering our community with continuous learning support.

## Built-in container security from code to production

In an era where software supply chain security is essential, Docker has raised the bar on container security. With integrated security measures across every phase of the development lifecycle, Docker helps teams build, test, and deploy confidently.

### Proactive security insights with Docker Scout Health Scores

Docker Scout, launched in 2023,  has become a cornerstone of Docker’s security ecosystem, empowering developer teams to identify and address vulnerabilities in container images early in the development lifecycle. By integrating with Docker Hub, Docker Desktop, and CI/CD workflows, Scout ensures that security is seamlessly embedded into every build. 

Addressing vulnerabilities during the inner loop — the development phase — is estimated to be up to 100 times less costly than fixing them in production. This underscores the critical importance of early risk visibility and remediation for engineering teams striving to deliver secure, production-ready software efficiently.

In 2024, we announced Docker Scout Health Scores (Figure 3), a feature designed to better communicate the security posture of container images development teams use every day. Docker Scout Health Scores provide a clear, alphabetical grading system (A to F) that evaluates common vulnerabilities and exposures (CVEs) for software components within Docker Hub. This feature allows developers to quickly assess and wisely choose trusted content for a secure software supply chain. 

<figure>

![creenshot of Docker Scout health score page showing checks for high profile vulnerabilities, Supply chain attestations, unapproved images, outdated images, and more.](https://www.docker.com/wp-content/uploads/2024/12/F3-Scout-health-score-1110x896.png "creenshot of Docker Scout health score page showing checks for high profile vulnerabilities, Supply chain attestations, unapproved images, outdated images, and more. - F3 Scout health score")

<figcaption>

**Figure 3:** Docker Scout health score.

</figcaption>

</figure>

For a deeper dive, check out our blog post on enhancing container security with Docker Scout and secure repositories.

### Air-gapped containers: Enhanced security for isolated environments

Docker introduced support for air-gapped containers in Docker Desktop 4.31, addressing the unique needs of highly secure, offline environments. Air-gapped containers enable developers to build, run, and test containerized applications without requiring an active internet connection. 

This feature is crucial for organizations operating in industries with stringent compliance and security requirements, such as government, healthcare, and finance. By allowing developers to securely transfer container images and dependencies to air-gapped systems, Docker simplifies workflows and ensures that even isolated environments benefit from the power of containerization.

### Strengthening trust with SOC 2 Type 2 and ISO 27001 certifications

Docker also achieved two major milestones in its commitment to security and reliability: SOC 2 Type 2 attestation and ISO 27001 certification. These globally recognized standards validate Docker’s dedication to safeguarding customer data, maintaining robust operational controls, and adhering to stringent security practices. SOC 2 Type 2 attestation focuses on the effective implementation of security, availability, and confidentiality controls, while ISO 27001 certification ensures compliance with best practices for managing information security systems.

These certifications provide developers and organizations with increased confidence in Docker’s ability to support secure software supply chains and protect sensitive information. They also demonstrate Docker’s focus on aligning its services with the needs of modern enterprises.

## Accelerating success for development teams and organizations

In 2024, Docker introduced a range of features and enhancements designed to empower development teams and streamline operations across organizations. From harnessing the potential of AI to simplifying deployment workflows and improving security, Docker’s advancements are focused on enabling teams to work smarter and build with confidence. By addressing key challenges in development, management, and security, Docker continues to drive meaningful outcomes for developers and businesses alike.

### Docker Home: A central hub to access and manage Docker products

Docker introduced Docker Home (Figure 4), a central hub for users to access Docker products, manage subscriptions, adjust settings, and find resources — all in one place. This approach simplifies navigation for developers and admins. Docker Home allows admins to manage organizations, users, and onboarding processes, with access to dashboards for monitoring Docker usage.

Future updates will add personalized features for different roles, and business subscribers will gain access to tools like the Docker Support portal and organization-wide notifications.

<figure>

![Screenshot of Docker Home showing options to explore Docker products, Admin console, and more.](https://www.docker.com/wp-content/uploads/2024/12/F4-Docker-home-1088x1024.png "Screenshot of Docker Home showing options to explore Docker products, Admin console, and more. - F4 Docker home")

<figcaption>

**Figure 4:** Docker Home.

</figcaption>

</figure>

### Empowering AI innovation  

Docker’s ecosystem supports AI/ML workflows, helping developers work with these cutting-edge technologies while staying cloud-native and agile. Read the Docker Labs GenAI series to see how we’re innovating and experimenting in the open.

Through partnerships like those with NVIDIA and GitHub, Docker ensures seamless integration of AI tools, allowing teams to rapidly experiment, deploy, and iterate. This emphasis on enabling advanced tech aligns Docker with organizations looking to leverage AI and ML in containerized environments.

#### Optimizing AI application development with Docker Desktop and NVIDIA AI Workbench

Docker and NVIDIA partnered to integrate Docker Desktop with NVIDIA AI Workbench, streamlining AI development workflows. This collaboration simplifies setup by automatically installing Docker Desktop when selected as the container runtime in AI Workbench, allowing developers to focus on creating, testing, and deploying AI models without configuration hassles. By combining Docker’s containerization capabilities with NVIDIA’s advanced AI tools, this integration provides a seamless platform for model training and deployment, enhancing productivity and accelerating innovation in AI application development. 

#### Docker + GitHub Copilot: AI-powered developer productivity

We announced that Docker joined GitHub’s Partner Program and unveiled the Docker extension for GitHub Copilot (@docker). This extension is designed to assist developers in working with Docker directly within their GitHub workflows. This integration extends GitHub Copilot’s technology, enabling developers to generate Docker assets, learn about containerization, and analyze project vulnerabilities using Docker Scout, all from within the GitHub environment.

#### Accelerating AI development with the Docker AI catalog

Docker launched the AI Catalog, a curated collection of generative AI images and tools designed to simplify and accelerate AI application development. This catalog offers developers access to powerful models like IBM Granite, Llama, Mistral, Phi 2, and SolarLLM, as well as applications such as JupyterHub and H2O.ai. By providing essential tools for machine learning, model deployment, inference optimization, orchestration, ML frameworks, and databases, the AI Catalog enables developers to build and deploy AI solutions more efficiently. 

The Docker AI Catalog addresses common challenges in AI development, such as decision overload from the vast array of tools and frameworks, steep learning curves, and complex configurations. By offering a curated list of trusted content and container images, Docker simplifies the decision-making process, allowing developers to focus on innovation rather than setup. This initiative underscores Docker’s commitment to empowering developers and publishers in the AI space, fostering a more streamlined and productive development environment. 

### Streamlining enterprise administration 

#### Simplified deployment and management with Docker’s MSI and PKG installers

Docker simplifies deploying and managing Docker Desktop with the new MSI Installer for Windows and PKG Installer for macOS. The MSI Installer enables silent installations, automated updates, and login enforcement, streamlining workflows for IT admins. Similarly, the PKG Installer offers macOS users easy deployment and management with standard tools. These installers enhance efficiency, making it easier for organizations to equip teams and maintain secure, compliant environments.

These new installers also align with Docker’s commitment to simplifying the developer experience and improving organizational management. Whether you’re setting up a few machines or deploying Docker Desktop across an entire enterprise, these tools provide a reliable and efficient way to keep teams equipped and ready to build.

#### New sign-in enforcement options enhance security and help streamline IT administration 

Docker simplifies IT administration and strengthens organizational security with new sign-in enforcement options for Docker Desktop. These features allow organizations to ensure users are signed in while using Docker, aligning local software with modern security standards. With flexible deployment options — including macOS Config Profiles, Windows Registry Keys, and the cross-platform registry.json file — IT administrators can easily enforce policies that prevent tampering and enhance security. These tools empower organizations to manage development environments more effectively, providing a secure foundation for teams to build confidently.

#### Desktop Insights: Unlocking performance and usage analytics

Docker introduced Desktop Insights, a powerful feature that provides developers and teams with actionable analytics to optimize their use of Docker Desktop. Accessible through the Docker Dashboard, Desktop Insights offers a detailed view of resource usage, build times, and performance metrics, helping users identify inefficiencies and fine-tune their workflows (Figure 5).

Whether you’re tracking the speed of container builds or understanding how resources like CPU and memory are being utilized, Desktop Insights empowers developers to make data-driven decisions. By bringing transparency to local development environments, this feature aligns with Docker’s mission to streamline container workflows and ensure developers have the tools to build faster and more effectively.

<figure>

![Screenshot of Docker Insights within Admin console, showing data for Total active users, Users with license, Total Builds, Total Containers run, and more](https://www.docker.com/wp-content/uploads/2024/12/F5-Desktop-insights-1110x936.png "Screenshot of Docker Insights within Admin console, showing data for Total active users, Users with license, Total Builds, Total Containers run, and more - F5 Desktop insights")

<figcaption>

**Figure 5:** Desktop Insights dashboard.

</figcaption>

</figure>

#### New usage dashboards in Docker Hub

Docker introduced Usage dashboards in Docker Hub, giving organizations greater visibility into how they consume resources. These dashboards provide detailed insights into storage and image pull activity, helping teams understand their usage patterns at a granular level (Figure 6). 

By breaking down data by repository, tag, and even IP address, the dashboards make it easy to identify high-traffic images or repositories that might require optimization. With this added transparency, teams can better manage their storage, avoid unnecessary pull requests, and optimize workflows to control costs. 

Usage dashboards enhance accountability and empower organizations to fine-tune their Docker Hub usage, ensuring resources are used efficiently and effectively across all projects.

<figure>

![Screenshot of Docker Usage dashboard showing a graph of daily pulls over time.](https://www.docker.com/wp-content/uploads/2024/12/F6-Usage-dashboard-1110x625.png "Screenshot of Docker Usage dashboard showing a graph of daily pulls over time. - F6 Usage dashboard")

<figcaption>

**Figure 6:** Usage dashboard.

</figcaption>

</figure>

#### Enhancing security with organization access tokens

Docker introduced organization access tokens, which let teams manage access to Docker Hub repositories at an organizational level. Unlike personal access tokens tied to individual users, these tokens are associated with the organization itself, allowing for centralized control and reducing reliance on individual accounts. This approach enhances security by enabling fine-grained permissions and simplifying the management of automated processes and CI/CD pipelines. 

Organization access tokens offer several advantages, including the ability to set specific access permissions for each token, such as read or write access to selected repositories. They also support expiration dates, aligning with compliance requirements and bolstering security. By providing visibility into token usage and centralizing management within the Admin Console, these tokens streamline operations and improve governance for organizations of all sizes. 

## Docker’s vision for 2025

Docker’s journey doesn’t end here. In 2025, Docker remains committed to expanding its support for cloud-native and AI/ML development, reinforcing its position as the go-to container platform. New integrations and expanded multi-cloud capabilities are on the horizon, promising a more connected and versatile Docker ecosystem.

As Docker continues to build for the future, we’re committed to empowering developers, supporting the open source community, and driving efficiency in software development at scale. 

2024 was a year of transformation for Docker and the developer community. With major advances in our product suite, continued focus on security, and streamlined experiences that deliver value, Docker is ready to help developer teams and organizations succeed in an evolving tech landscape. As we head into 2025, we invite you to **explore Docker’s suite of tools** and see how Docker can help your team build, innovate, and secure software faster than ever.

## Learn more

- Read the Docker Desktop release collection to see even more updates and innovation announcements.
- Subscribe to the Docker Navigator newsletter.
- Discover the upgraded Docker plans. 
- Find out how signing in unlocks advanced features.
- See what’s new in Docker Desktop.

​Products, AI/ML, Docker, Docker Debug, Docker Desktop, Docker Hub, Docker Scout, security, What Is Docker collection
