# FSx
[FSx FAQ](https://aws.amazon.com/fsx/windows/faqs/)

FSx comes in two flavors:
- FSx for Windows
- FSx for Lustre

## FSx for Windows
Fully managed native Microsoft Windows filesystem so you can easily move Windows-based apps that require file storage to AWS. FSx is built on Windows Server.

Examples:
- SQL Server
- IIS
- SharePoint Server

Features:
- FSx is a managed windows server that runs Windows Server Message Block (SMB)-based file services.
- Designed for Windows and Windows Applications
- Supports AD users, access control lists, and security policies
- Supports Distributed File System (DFS) namespaces and replication.

>Note: If an exam question asks about SMB storage, FSx is the correct choice.

## FSx for Lustre
Fully managed file system that is optimized for compute-intensive workloads such as high-performance computing (HPC), machine learning, media data processing, and electronic design automation (EDA).

Lustre filesystems can processes hundreds of gigabytes per second, millions of IOPS, and sub-millisecond latencies.

>Note: If an exam question asks about high-speed, high-capacity distributed storage use FSx for Lustre. FSx for Lustre can store data directly on S3.
