---
title: "Issue with RedShift JDBC Driver Python Connector and ODBC Driver - CVE-2024-12744 CVE-2024-12745 CVE-2024-12746"
date: Tue, 24 Dec 2024 18:04:59 +0000
draft: false
type: posts
categories: 
- 
---
# Issue with RedShift JDBC Driver Python Connector and ODBC Driver - CVE-2024-12744 CVE-2024-12745 CVE-2024-12746

<br/>

<br/>
**Publication Date: 2024/12/24 10:00AM PST**  

AWS has identified the following issues within the Amazon Redshift JDBC Driver, Amazon Redshift Python Connector, and Amazon Redshift ODBC Driver. On December 23, 2024, we released a fix and recommend customers upgrade to the latest version to address these issues.

-   The Amazon Redshift JDBC Driver, version 2.1.0.31, is affected by [CVE-2024-12744](https://www.cve.org/CVERecord?id=CVE-2024-12744), a SQL injection issue when utilizing the getSchemas, getTables, or getColumns Metadata APIs. This issue has been addressed in driver version 2.1.0.32. We recommend customers upgrade to the driver version 2.1.0.32 or revert to driver version 2.1.0.30.
-   The Amazon Redshift Python Connector, version 2.1.4, is affected by [CVE-2024-12745](https://www.cve.org/CVERecord?id=CVE-2024-12745), a SQL injection issue when utilizing the get\_schemas, get\_tables, or get\_columns Metadata APIs. This issue has been addressed in driver version 2.1.5. We recommend customers upgrade to the driver version 2.1.5 or revert to driver version 2.1.3.
-   The Amazon Redshift ODBC Driver, version v2.1.5.0 (Windows or Linux), is affected by [CVE-2024-12746](https://www.cve.org/CVERecord?id=CVE-2024-12746), a SQL injection issue when utilizing the SQLTables or SQLColumns Metadata APIs. This issue has been addressed in driver version 2.1.6.0. We recommend customers upgrade to the driver version 2.1.6.0 or revert to driver version 2.1.4.0.

**Affected versions:** Amazon Redshift JDBC Driver, version 2.1.0.31; Amazon Redshift Python Connector, version 2.1.4; Amazon Redshift ODBC Driver, version v2.1.5.0.

**Resolution:**  
Users of the Amazon Redshift JDBC Driver should upgrade to the driver version 2.1.0.32 or revert to driver version 2.1.0.30.

Users of the Amazon Redshift Python Connector are recommended to upgrade to the driver version 2.1.5 or revert to driver version 2.1.3.

Users of the Amazon Redshift ODBC Driver are recommended to upgrade to the driver version 2.1.6.0 or revert to driver version 2.1.4.0.

**References:**

-   CVE-2024-12744 [GitHub Security Advisory](https://github.com/aws/amazon-redshift-jdbc-driver/security/advisories/GHSA-8596-2jgr-ppj7)
-   CVE-2024-12745 [GitHub Security Advisory](https://github.com/aws/amazon-redshift-python-driver/security/advisories/GHSA-8gc2-vq6m-rwjw)
-   CVE-2024-12746 [GitHub Security Advisory](https://github.com/aws/amazon-redshift-odbc-driver/security/advisories/GHSA-g63m-5vjv-wr3v)

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2024-015/)

<br/>
---
