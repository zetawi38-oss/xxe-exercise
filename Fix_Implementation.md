# XXE Fix Implementation Guide

## 1. Introduction

This document describes the practical steps required to apply the recommended security fix for the XXE vulnerability in the Flask application.

The original application is intentionally vulnerable and is kept unchanged for the original vulnerability demonstration.

## 2. Original Vulnerable Configuration

The original parser configuration in `app.py` is:

```python
parser = etree.XMLParser(
    no_network=False,
    resolve_entities=True
)
```

This configuration allows external entity resolution and does not disable network access.

## 3. Secure Configuration

The recommended replacement is:

```python
from lxml import etree

parser = etree.XMLParser(
    resolve_entities=False,
    load_dtd=False,
    no_network=True,
    huge_tree=False
)
```

## 4. Implementation Steps

1. Open the application source file `app.py`.
2. Locate the original XMLParser configuration.
3. Replace the vulnerable configuration with the secure configuration.
4. Save the changes.
5. Rebuild and restart the Docker application.
6. Test normal XML parsing.
7. Repeat the external entity tests to verify that local file disclosure is prevented.

## 5. Expected Results

- Normal XML input should continue to work.
- External entity resolution should be disabled.
- External DTD loading should be disabled.
- The application should not disclose local file contents through XXE.

## 6. Project Status

The original vulnerable application has not been modified.

This document describes the recommended implementation procedure. The secure configuration has not yet been applied or tested.

## 7. Conclusion

Keeping the vulnerable application unchanged preserves the original proof of concept.

The implementation guide provides the steps required to apply and verify the security fix separately.
