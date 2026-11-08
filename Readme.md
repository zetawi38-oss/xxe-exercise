# XXE Vulnerability

## 1. Introduction

This project focuses on the XML External Entity (XXE) vulnerability in a vulnerable web application.

The objective is to understand how XXE works, identify the vulnerability, demonstrate its impact, and learn how to protect web applications from this type of attack.

## 2. What is XXE?

XXE stands for XML External Entity.

It is a security vulnerability that occurs when an application processes XML input containing external entities without proper security restrictions.

An attacker may exploit this vulnerability to make the application access local files or external resources.

## 3. Application Description

The application is a web application built using Python and Flask.

It provides an XML parsing function that accepts XML input from users and processes it using the lxml library.

The application was started using Docker.

The local application was accessed through:

`http://localhost:5000`

## 4. Vulnerability Identification

After examining the application source code in `app.py`, the following XML parser configuration was identified:

```python
parser = etree.XMLParser(
    no_network=False,
    resolve_entities=True
)
```

The `resolve_entities=True` setting allows XML entities to be resolved.

The `no_network=False` setting does not disable network access.

These settings create an unsafe XML parsing configuration when processing untrusted user input.

## 5. Testing and Exploitation

First, a normal XML document was submitted to verify that the application could parse XML successfully.

After that, an external entity test was performed using the following local file:

`/etc/passwd`

The application successfully displayed the contents of the file.

A second test was performed using:

`/etc/hostname`

The application successfully displayed the hostname of the container.

These results confirmed that the application was vulnerable to XXE.

## 6. Attack Scenarios

### Scenario 1: Local File Disclosure

An attacker may use an external entity to access local files that the application process has permission to read.

This may expose sensitive information, configuration files, and system account information.

In this project, this scenario was demonstrated by reading `/etc/passwd` and `/etc/hostname`.

### Scenario 2: Server-Side Request Forgery (SSRF)

An attacker may exploit external entity processing to make the server send requests to internal network resources.

This could expose internal services or information that is not directly accessible from the internet.

This scenario is a potential impact of XXE and was not tested in this project.

## 7. Security Recommendations

To prevent XXE vulnerabilities:

- Disable external entity resolution.
- Disable loading external DTDs when they are not required.
- Disable network access from the XML parser.
- Validate and restrict user-supplied XML input.
- Use secure XML parser configurations.
- Run the application with minimum necessary permissions.

## 8. Conclusion

The project demonstrated an XML External Entity vulnerability in the training application.

The vulnerability was caused by unsafe XML parser configuration.

The successful tests showed that external entities could be used to disclose local files from the application environment.

This project helped demonstrate the importance of secure XML parsing and proper input validation.
