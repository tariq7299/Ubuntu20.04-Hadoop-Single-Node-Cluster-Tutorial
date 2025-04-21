# Hadoop-Ranger-LDAP Single Node Cluster Setup

This repository provides a comprehensive step-by-step guide to setting up a complete Apache Hadoop single-node cluster with Apache Ranger for security management and LDAP integration for authentication, all running on Ubuntu 20.04.

## Overview

Setting up a secure Hadoop environment with proper authentication and authorization can be challenging. This guide aims to simplify the process by providing detailed instructions for creating a fully functional development environment that demonstrates enterprise-level security practices.

## Features

- Complete Apache Hadoop 2.x single-node cluster setup
- Apache Ranger integration for centralized security administration
- OpenLDAP configuration for user authentication
- Step-by-step installation process with explanations
- Troubleshooting tips for common issues

## Prerequisites

- Ubuntu 20.04 LTS (Server or Desktop)
- Minimum 8GB RAM (16GB recommended)
- At least 50GB free disk space
- Root or sudo access
- Basic knowledge of Linux commands

## Installation Steps

### 1. System Preparation

- Setting up the Ubuntu environment
- Installing required dependencies
- Configuring hostname and network settings
- Creating necessary user accounts

### 2. Hadoop Installation

- Downloading and extracting Hadoop
- Configuring core-site.xml, hdfs-site.xml, mapred-site.xml, and yarn-site.xml
- Setting up SSH key-based authentication
- Formatting HDFS namenode
- Starting HDFS and YARN services

### 3. Apache Ranger Installation

- Prerequisites for Ranger
- Database setup (MySQL)
- Configuring Ranger Admin service
- Setting up Ranger plugins for HDFS and YARN
- Initializing Ranger policies

### 4. LDAP Configuration

- Installing and configuring OpenLDAP
- Creating organizational units and users
- Setting up LDAP authentication for Hadoop
- Integrating LDAP with Ranger for user synchronization

### 5. Testing the Setup

- Verifying HDFS operations with different users
- Testing Ranger policy enforcement
- Validating LDAP authentication

## Troubleshooting

Common issues and their solutions:
- Hadoop service startup failures
- Ranger policy synchronization problems
- LDAP connection issues
- Permission-related errors

## Resources

- Official Apache Hadoop documentation
- Apache Ranger user guide
- OpenLDAP administration references
- Helpful community resources

## Contributing

Contributions to improve this guide are welcome! Please feel free to submit pull requests or open issues if you find areas that need clarification or enhancement.

---

*Note: This guide is intended for development and learning purposes. For production environments, additional considerations for high availability, performance tuning, and security hardening would be necessary.*  


# development accounts info  

Admin username of the ubuntu VM is : tariq  
Admin Password of the ubuntu VM is : 1122
  
Hadoop username: hdoop  
Hadoop Password: 1122

ResourceManager WebInterface Access on: localhost:8088  
NameNode WebInterface Access on: localhost:9870  

Root user of MYSQL is : root  
Password: 1122  

Admin user of MYSQL: rangeradmin  
password: 1122 

User who can start and stop solr: solr  
password: 1122  

User who can start and stop Ranger Admin: ranger  
password: 1122  

Ldap user admin username: admin  
password: 1122  

LDAP user: hdoop
password: 1122  

