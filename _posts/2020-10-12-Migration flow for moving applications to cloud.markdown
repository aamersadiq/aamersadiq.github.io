---
title:  "Migration flow for moving applications to cloud"
date:   2020-10-12 11:45:43
categories: [migration]
tags: [migration]	
---
<p>
If you're looking to transition your current applications from on-premises to the cloud, or from one cloud to another, there are certain steps you can take to ensure a seamless migration. In this blog post, we'll go over these concrete steps to help you plan and execute your migration with minimal disruptions.
<img src="{{ site.baseurl }}/images/blog/migration-flow/migration-and-consolidation-flow.PNG" class="fullsize-image" alt="Image">
</p>

<p>
When it comes to migrating your applications, it's important to have a clear and organized plan in place. One way to do this is by dividing the process into these three distinct phases:
<ul>
<li>Concept - Planning and preparation</li>
<p>This phase is all about gathering the necessary information about your applications, identifying potential roadblocks, and creating a detailed project plan. By taking the time to thoroughly plan and prepare, you'll be setting yourself up for a smoother migration process.</p>
<li>Implementation - Execution</li>
<p>This phase is where the actual migration takes place. This could include tasks such as configuring the new infrastructure, deploying the applications, and testing to make sure everything is working as expected.</p>
<li>Completion - Post migration</li>
<p>After the migration is complete, it's important to monitor the applications in the new environment to ensure everything is running smoothly. This phase also includes addressing any issues that arise, training users and administrators, and documenting the entire migration process for future reference</p>
</ul>
</p>
<p>Lets look at these three phases in detail.</p>
<h3>1. Concept Phase</h3>
Concept phase is divided into these activities:
<h4>Discovery and Analysis</h4>
<p>
When planning a migration, the first step is to gather all the information about the applications that will be moving to the new environment. One way to do this is by creating an application details document for each application. This document should include information such as:
<ul>
<li>Application name and version</li>
<li>Number of users and usage patterns</li>
<li>Business purpose and function</li>
<li>Contact information for vendor, business owner and technical owner along with their contact details</li>
<li>High level component diagrams exhibiting component type and which server it is hosted e.g web application components can be web site, caching layer, api, database and each could be hosted on different servers</li>
<li>Integration diagrams outlining the communication or dependency link between the given application and all the other applications</li>
<li>All the security accounts required for the application e.g. database logins, service accounts</li>
<li>Key resource utilization (CPU/memory/network utilization)</li>
<li>Performance benchmarks</li>
<li>Known issues</li>
</ul>
</p>
<h4>Current Landscape</h4>
<p>
Current landscape is a one page architectural view of all the applications considered for migrations. It should also provide the current cost of running the application on the existing servers, including the cost of hardware and software. 
</p>
<h4>Migration Strategy</h4>
<p>
Migration strategy will outline why, how and when the applications will be migrated and should include:
<ul>
<li>Purpose or business case for migrations e.g. what benefits will materialize from migration</li>
<li>Decision tree (or list of eligibility criteria) for determining application acceptability for migration. e.g. vendor/development team is available to perform the migration activities, application detail and support documents are available to facilitate and troubleshoot migration, no impact on application licensing, no performance impact, no feature impact </li>
<li>Group the applications based on the architecture or deployed components. e.g. web application, database, services etc. Then define the generic approach for migrating each type of application. e.g. for databases, backup and restore</li>
<li>Provide the approach for migrating each group of application. e.g. for databases, do backup and restore</li>
<li>Provide testing approach for each group of application. e.g. load testing for databases, regression test for web application</li>
<li>Proposed list of applications for migration</li>
<li>Final list of applications for migration which met the eligibility criteria</li>
<li>Define risk and effort for migrating each application. The risk can comprise of size or complexity of the application and business loss in event of unavailability. Drive effort from time required to prepare, migrate and test the application.</li>
<li>Outline the migration plan with time frame of each application migration. Risk and effort will have input into devised plan. e.g. Low risk and small effort applications are migrated first or vice versa.</li>
<li>Acceptance criteria for successful migration e.g. all features are working, integration with other application functional</li>
</ul>
</p>
<h4>Consolidation Strategy</h4>
<p>
During the migration, there might be opportunities to host multiple applications, databases on single instance of the server. To evaluate and execute these opportunities, consolidation strategy should include:
<ul>
<li>Purpose or business case for consolidation e.g. what benefits will materialize from consolidation</li>
<li>Materialized benefits of consolidation, saving in terms or dollars. Consider capturing number of CPUs and memory used before and after consolidations, and use it to calculate savings in dollars </li>
</ul>
</p>
<h4>Future Landscape</h4>
<p>
Future landscape is a one page architectural view of all the applications after the migration. It should provide the cost of running the application on the new servers, including the cost of hardware and software. 
</p>

<h3>2. Implementation Phase</h3>
Implementation phase is divided into these activities:
<h4>Pre Migration Checklist</h4>
<p>
Checklist per application which include:
<ul>
<li>Is migration date finalized? (Record the date of migration)</li>
<li>Are business stakeholders engaged and communicated? (Record stakeholders name and type of communication sent)</li>
<li>Are vendor/developer/support teams availability checked and their times are booked?</li>
<li>Is application detail document available? (Completed during Concept phase)</li>
<li>Is application support document available? (Acquired from support team/vendor)</li>
<li>Is deployment/rollback plan available? (Can be completed during Concept or Implementation phase)</li>
<li>Is test plan available? (Can be completed during Concept or Implementation phase)</li>
</ul>
</p>
<h4>Migrate/Test</h4>
<p>
This is the step when application is migrated to the new environment. In spite of all the planning, problems may arise during the migration and atmosphere can become chaotic. During those times, use the support team and helping documents to bring calm to the chaos. Execute testing according to test plan to ensure application is intact and all features are working.
</p>
<h4>Post Migration Checklist</h4>
<p>
Checklist per application which include:
<ul>
<li>Is migration completed successfully? (Record the date of migration)</li>
<li>Has stakeholders signed off on the successful migration? (Record the names and sign off date)</li>
<li>List any follow up tasks? (Server names to decommission , post migration review to be scheduled)</li>
</ul>
</p>
<h3>3. Completion Phase</h3>
Completion phase is divided into various activities:
<h4>Decommission Old Infrastructure</h4>
Once all the applications are successfully migrated, switch off old servers. It is recommended to completely decommission infrastructure after 60 days of no issue reported.
<h4>Reporting and Metrics</h4>
<p>
Create reports to capture:
<ul>
<li>What went well and any issues faced during migrations?</li>
<li>Any learning which be applied to next migration</li>
<li>Compare the evident benefit against the proposed benefit</li>
<li>Capture performance metrics for migrated application and compare against the pre-migration performance metrics</li>
</ul>
</p>


