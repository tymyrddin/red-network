# Active directory overview

The Active Directory structure includes three main tiers: 1) domains, 2) trees, and 3) forests. Several objects (users or devices) that all use the same database may be grouped in to a single domain. Multiple domains can be combined into a single group called a tree. Multiple trees may be grouped into a collection called a forest. Each one of these levels can be assigned specific access rights and communication privileges.

## Main concepts of an Active Directory

* Directory – Contains all the information about the objects of the Active directory
* Object – An object references almost anything inside the directory (a user, group, shared folder...)
* Domain – The objects of the directory are contained inside the domain. Inside a "forest" more than one domain can 
exist and each of them will have their own objects collection.
* Tree – Group of domains with the same root. Example: dom.local, email.dom.local, www.dom.local
* Forest – The forest is the highest level of the organization hierarchy and is composed by a group of trees. The 
trees are connected by trust relationships.

Active Directory provides different services, which fall under the umbrella of Active Directory Domain Services (AD DS):

* Domain Services – stores centralized data and manages communication between users and domains; includes login authentication and search functionality
* Certificate Services – creates, distributes, and manages secure certificates
* Lightweight Directory Services – supports directory-enabled applications using the open (LDAP) protocol
* Directory Federation Services – provides single-sign-on (SSO) to authenticate a user in multiple web applications in a single session
* Rights Management – protects copyrighted information by preventing unauthorized use and distribution of digital content
* DNS Service – Used to resolve domain names.

AD DS is included with Windows Server (including Windows Server 10) and is designed to manage client systems. While 
systems running the regular version of Windows do not have the administrative features of AD DS, they do support 
Active Directory. This means any Windows computer can connect to a Windows workgroup, provided the user has the 
correct login credentials.