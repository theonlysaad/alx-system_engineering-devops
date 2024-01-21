Postmortem: Web Stack Outage

Issue Summary:

    Duration: January 20, 2024, 14:30 - 18:45 (UTC)
    Impact: Users experienced a 45% increase in response times across our web application. The service downtime affected all geographical regions.
    Root Cause: A database connection leak resulted in resource exhaustion and degraded performance.

Timeline:

    Detection Time: January 20, 2024, 14:30 (UTC)
    Detection Method: An automated monitoring alert triggered due to an abnormal spike in response times.
    Actions Taken:
        14:35: Initial investigation focused on application servers and load balancers.
        15:00: Assumed it might be a network issue and escalated to the networking team.
        15:30: No issues found in the network; redirected the investigation to database servers.
        16:15: Database team identified a high number of open connections, suspected a potential connection leak.
        17:00: Realized that the issue was database-related and involved a connection leak.
    Misleading Paths:
        Initially, we assumed a network problem due to increased response times, leading to wasted time in the networking investigation.
        The focus on application servers delayed the identification of the actual root cause in the database layer.
    Escalation:
        16:30: Incident escalated to the database administration team.
        17:45: Executives were notified as user impact continued to escalate.

Root Cause and Resolution:

    Root Cause: A misconfiguration in the connection pooling settings of the database allowed connections to remain open indefinitely, causing a connection leak.
    Resolution: The database team reconfigured connection pooling settings to ensure timely closure of idle connections. Additionally, a script was implemented to regularly audit and close lingering connections.

Corrective and Preventative Measures:

    Improvements/Fixes:
        Implement automated checks for connection pool configurations during deployment.
        Enhance monitoring to alert on abnormal database connection patterns.
        Conduct regular audits on critical system configurations.
    Tasks:
        Database Team:
            Update connection pool configurations in all environments.
            Implement automated periodic audits for identifying and closing lingering connections.
        Monitoring Team:
            Enhance monitoring to include database connection metrics.
            Set up alerts for abnormal connection patterns.
        DevOps/Deployment Team:
            Ensure that deployment scripts include checks for correct connection pool configurations.
        Documentation Team:
            Update documentation to include best practices for database connection management.

Conclusion:
The outage, lasting approximately 4 hours, resulted from a database connection leak. Our initial focus on networking and application servers delayed the identification of the root cause. Implementing automated checks, enhancing monitoring, and conducting regular audits will fortify our system against similar incidents in the future. The swift resolution of this outage is credited to the collaborative efforts of the database, networking, and application teams.
