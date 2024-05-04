List of Topics:
- Only 1 will be selected for actual event.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
*01:* AWS Web Application Architecture Challenge

*Problem Statement:*

Your company aims to develop a scalable and resilient web application to provide a platform for users to collaborate on projects in real-time. The application should be capable of handling a growing user base while maintaining high availability and performance. As a cloud architect, your task is to design and implement the architecture for this web application using AWS services, ensuring security, scalability, and cost-effectiveness.

*Requirements:*

1. *Scalability:* Design an architecture that can handle varying levels of user traffic without compromising performance. The solution should be able to scale both vertically and horizontally based on demand.

2. *High Availability:* Ensure the web application is highly available to users. Design for fault tolerance and implement mechanisms to minimize downtime in the event of failures.

3. *Security:* Implement robust security measures to protect the web application and user data. Utilize AWS security services and best practices to prevent unauthorized access and mitigate potential security threats.

4. *Cost Optimization:* Design the architecture with cost optimization in mind. Utilize AWS services efficiently to minimize infrastructure costs while meeting performance and scalability requirements.

5. *Data Management:* Implement a data management strategy that ensures data consistency, durability, and scalability. Choose appropriate AWS database services and caching mechanisms to store and retrieve data efficiently.

6. *Monitoring and Logging:* Set up monitoring and logging solutions to track the performance and health of the web application. Utilize AWS monitoring services to gather insights into application behavior and detect potential issues proactively.

7. *Deployment Automation:* Implement automated deployment pipelines to streamline the deployment process and ensure consistency across environments. Utilize AWS DevOps services to automate the build, test, and deployment phases.

8. *Compliance and Governance:* Ensure compliance with industry standards and regulatory requirements. Implement governance policies and access controls to maintain security and compliance posture.

*Deliverables:*

- Architectural diagram detailing the components and their interactions.
- Description of each component and its purpose in the architecture.
- Explanation of how the architecture meets each requirement.

*Evaluation Criteria:*

- Does it adhere to AWS best practices and architectural principle?
- How scalable is this solution and its impact on performance of the proposed architecture?
- What are the fault tolerance measures implemented?
- What type of security measures and compliance are enforced?
- Is the solution designed with cost optimization in mind?

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

*02*: AWS Inventory Insights and Reporting Transformation Challenge

*Problem Statement:*

You are tasked with replacing the existing ServiceNow plugin used for inventory insights. The goal is to architect a solution that not only provides comprehensive inventory insights but also establishes a robust reporting mechanism. The solution should minimize technical debt and identify opportunities for cost savings.

*Requirements:*

1. *Data Transformation and Storage:*
   - Implement a mechanism to transform raw inventory data into a standardized format.
   - Choose appropriate AWS services for storing the transformed data securely and cost-effectively.

2. *Real-time Insights:*
   - Develop a system that provides real-time insights into the AWS inventory.

3. *Reporting and Visualization:*
   - Create a reporting mechanism that serves as the company's source of truth for AWS inventory.
   - Utilize AWS services or third-party tools for visualization and reporting.

5. *Cost Optimization:*
   - Identify areas for cost optimization within the AWS infrastructure.
   - Propose strategies or tools to reduce unnecessary expenses based on inventory insights.

6. *Security and Compliance:*
   - Implement security best practices to protect sensitive inventory data.
   - Ensure compliance with relevant industry regulations (e.g., GDPR, HIPAA).
  
*Evaluation Criteria:*

- Is the solution well-architected?
- How effectively does the solution collect, transform, and store inventory data?
- Does the solution provide timely and accurate insights into the AWS inventory?
- Is the reporting mechanism robust and user-friendly?
- Are cost-saving opportunities identified and effectively addressed?
- How well does the solution address security and compliance requirements?

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

*03:* AWS Automation of Managed Service Provider Tasks

*Problem Statement:*

As AWS organization owner, you are tasked with automating routine tasks (commonly performed by MSP) to improve operational efficiency and reduce manual intervention. These tasks include monthly patching, vault backup, and cleanup of unused resources. Your solution should leverage AWS services and include robust monitoring, logging, and alerting mechanisms.

*Requirements:*

1. *Task Automation:*
   - Design an automated solution to perform monthly patching, vault backup, and deletion of unused resources.
   - Ensure reliability, scalability, and consistency in task execution across environments.

2. *AWS Service Utilization:*
   - Utilize AWS services effectively to implement automation workflows.
   - Choose suitable services for each task considering scalability, reliability, and cost-effectiveness.

3. *Monitoring and Logging:*
   - Implement comprehensive monitoring and logging to track task execution and identify issues.
   - Ensure visibility into task statuses, errors, and performance metrics.

4. *Alerting Mechanism:*
   - Configure alerting mechanisms to notify stakeholders promptly of task failures or anomalies.
   - Utilize AWS services for proactive alerting and efficient notification management.

5. *Security and Compliance:*
   - Implement security best practices to safeguard sensitive data and resources involved in automated tasks.
   - Ensure compliance with relevant industry standards and regulations.

*Evaluation Criteria:*

- Is the solution well-architected and scalable for automating routine MSP tasks?
- Have appropriate AWS services been chosen for each task?
- Is there adequate visibility into task execution and performance?
- Are stakeholders promptly notified of task failures or anomalies?
- How well does the solution address security and compliance requirements?

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
*04:* AWS Automated Ticketing and Reporting System for AWS Inspector/Security Hub Findings

*Problem Statement:*

As part of your responsibilities as a cloud architect, you are tasked with designing an automated ticketing and reporting system for AWS Inspector/Security Hub findings. The system should assign a unique ticket number to each finding/event, associate it with the corresponding AWS account owner, and provide a reporting mechanism to track the status of each ticket. Additionally, notifications should be sent to account owners based on severity levels, and reminders should be scheduled to ensure timely closure of findings.

*Requirements:*

1. *Ticket Assignment:*
   - Design a system to assign a unique ticket number to every AWS Inspector/Security Hub finding/event.
   - Ensure that each ticket is associated with the corresponding AWS account owner.

2. *Reporting System:*
   - Implement a reporting mechanism to store the inventory of tickets along with their status (e.g., open, closed).
   - Provide functionalities for querying and updating ticket status.

3. *Notification System:*
   - Introduce a notification mechanism to notify AWS account owners of their findings.
   - Customize notifications based on severity levels and include instructions on how to close the findings.

4. *Reminder Functionality:*
   - Configure reminders to be sent to account owners based on severity levels with specified lead times.
   - Ensure reminders prompt account owners to take action on findings before specified deadlines.

*Evaluation Criteria:*

- Is the system well-architected and scalable for managing AWS Inspector/Security Hub findings?
- Are unique ticket numbers assigned to findings, and are they associated with the correct account owners?
- Does the reporting mechanism provide accurate inventory of tickets and their statuses?
- Are notifications and reminders configured effectively based on severity levels and lead times?
- Have appropriate AWS services been utilized effectively in the solution?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
*05:* AWS Event Management Web Application

*Problem Statement:*

You are tasked with designing a web application to help a company host an event attended by retailers across the US. The event will feature activities such as participant registration, attendance tracking, and a lucky draw at the end. The web app should facilitate event planning activities and provide features for attendees to register, provide personal information, specify food preferences, and receive notifications about the event agenda and lucky draw winners.

*Requirements:*

1. *Participant Registration:*
   - Design a registration feature for participants to sign up for the event.
   - Collect necessary information such as name, email, company name, and food preferences.

2. *Event Agenda and Notifications:*
   - Provide a feature to display the event agenda and send notifications to attendees about upcoming activities and announcements.
   - Ensure notifications are delivered via email or SMS to registered participants.

3. *Attendance Tracking:*
   - Implement functionality to track attendee check-ins and confirmations during the event.
   - Enable event organizers to view real-time attendance status and generate reports if necessary.

4. *Lucky Draw:*
   - Automate the selection of lucky draw winners based on the attendees list.
   - Notify winners and display announcements of lucky draw results during the event.

*Evaluation Criteria:*

- Is the architecture well-structured and scalable for hosting the event management web application?
- Are all required features, including participant registration, event agenda, attendance tracking, and lucky draw, implemented effectively?
- Have appropriate AWS services been utilized effectively in the solution?
- Are security measures implemented to protect participant data and ensure secure access to the application?
- Is the solution designed to handle potential spikes in traffic and ensure high availability during the event?
  
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
