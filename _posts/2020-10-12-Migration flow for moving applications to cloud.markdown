---
title:  "Migration flow for moving applications to cloud"
date:   2020-10-12 11:45:43
categories: [migration]
tags: [migration]	
---
<p>
If you are thinking to migrate your existing applications, from on-premises to cloud, or from cloud to another cloud, these are the concrete steps you can take to ensure everything goes down smoothly.
<img src="{{ site.baseurl }}/images/blog/migration-flow/migration-and-consolidation-flow.PNG" class="fullsize-image" alt="Image">
</p>
<p>
I have divided the whole process into three phases:
<ul>
<li>Concept: Or planning phase</li>
<li>Implementation: Where stuff is done</li>
<li>Completion: Review and close</li>
</ul>
</p>

<h3>1. Concept Phase</h3>
Concept phase is divided into various activities:
<h4>Discovery and Analysis</h4>
<p>
During discovery and analysis, collect as much information about the applications. I recommend creating a application details document per application to capture the following information:
<ul>
<li>Application name, version, number of active users</li>
<li>Contact information for vendor, business owner and technical owner along with their contact details</li>
<li>High level component diagram exhibiting component type and where it is hosted e.g web application components can be web site, caching layer, api, database and each could be hosted on different servers</li>
<li>Integration diagram outlining the communication or dependency link between the given application and all the other applications</li>
<li>All the security accounts required for the application e.g. database logins, service accounts</li>
<li>Key performance metrics (CPU/memory/network utilization)</li>
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
<li>Purpose or business case for migrations e.g. what benefits will materialise from migration</li>
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
Implementation phase is divided into various activities:
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
This is the day application is migrated. If planning is done effectively then migration will go smoothly. Execute testing according to test plan to ensure application is intact and working as before.
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


