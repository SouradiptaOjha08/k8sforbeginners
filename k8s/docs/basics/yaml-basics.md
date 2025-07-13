# YAML Primer for Beginners (For Ansible and Kubernetes)

## Table of Contents
1. [Introduction](#introduction)
2. [What is YAML?](#what-is-yaml)
3. [Key Characteristics](#key-characteristics)
4. [Data Types in YAML](#data-types-in-yaml)
   - [Scalars](#scalars)
   - [Lists](#lists)
   - [Dictionaries (Mappings)](#dictionaries-mappings)
   - [Nested Structures](#nested-structures)
5. [Indentation and Placement of Objects](#indentation-and-placement-of-objects)
6. [Example Kubernetes YAML File (20 lines)](#example-kubernetes-yaml-file-20-lines)
7. [Line-by-Line Breakdown](#line-by-line-breakdown)
8. [Additional Important YAML Concepts](#additional-important-yaml-concepts)
9. [More K8s YAML Examples](#more-k8s-yaml-examples)
10. [Common YAML Mistakes](#common-yaml-mistakes)
11. [Cheat Sheet Summary](#cheat-sheet-summary)
12. [Troubleshooting YAML Files](#troubleshooting-yaml-files)
13. [Conclusion](#conclusion)
14. [Additional Resources](#additional-resources)

---

## Introduction

YAML (YAML Ain't Markup Language) is a human-readable data serialization format. It is widely used in configuration management and orchestration tools such as **Ansible** and **Kubernetes**. This guide is designed for beginners who want to quickly grasp YAML fundamentals.

---

## What is YAML?

YAML is:

- Indentation-sensitive (uses spaces, not tabs).
- A structured format similar to JSON but more readable.
- Composed of key-value pairs, lists, and nested structures.

---

## Key Characteristics

- Case-sensitive
- No tabs allowed — **use spaces only**
- Indentation defines hierarchy
- Commonly 2 spaces per level (can also use 4, but be consistent)

---

## Data Types in YAML

### Scalars
Simple data types like strings, integers, floats, booleans.

```yaml
name: Kubernetes
version: 1.26
enabled: true
timeout: 30.5
````

---

### Lists

```yaml
items:
  - Ansible
  - Kubernetes
  - Docker
```

---

### Dictionaries (Mappings)

```yaml
service:
  name: web-service
  port: 80
  protocol: HTTP
```

---

### Nested Structures

```yaml
deployment:
  name: my-app
  replicas: 3
  containers:
    - name: app-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

---

## Indentation and Placement of Objects

* Use consistent spacing (commonly 2 spaces)
* Indentation represents parent-child relationships
* No tabs — use **spaces only**

---

## Example Kubernetes YAML File (20 lines)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

---

## Line-by-Line Breakdown

1. `apiVersion:` Kubernetes API version
2. `kind:` Resource type (Deployment)
3. `metadata:` Contains resource name and labels
4. `spec:` Describes desired state
5. `replicas:` Number of pod replicas
6. `selector:` Matches pods to manage
7. `matchLabels:` Defines label selector
8. `template:` Describes pod template
9. `metadata:` Labels inside template
10. `spec:` Container specs
11. `containers:` List of containers
12. `name:` Container name
13. `image:` Docker image
14. `ports:` List of ports
15. `containerPort:` Port exposed by container

---

## Additional Important YAML Concepts

### Comments

```yaml
# This is a comment
name: app-name  # Inline comment
```

---

### Multiline Strings

**Literal Block (`|`)** — preserves line breaks:

```yaml
description: |
  This is a line.
  This is another line.
```

**Folded Block (`>`)** — folded into a single line:

```yaml
description: >
  This will become a single
  line when parsed.
```

---

### Anchors and Aliases

```yaml
defaults: &app_defaults
  image: nginx
  pullPolicy: IfNotPresent

container:
  <<: *app_defaults
  name: web-app
```

---

### Booleans and Nulls

```yaml
enabled: true
debug: FALSE
value: null
unset: ~
```

---

### File Separation (Multiple Resources)

```yaml
# First object
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
---
# Second object
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
```

---

## More K8s YAML Examples

### ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  LOG_LEVEL: debug
  MAX_RETRIES: "5"
```

---

### Secret (Base64 Encoded)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

---

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

---

### Environment Variables

```yaml
env:
  - name: APP_ENV
    value: production
  - name: LOG_LEVEL
    value: debug
```

---

## Common YAML Mistakes

| Mistake                         | What Happens               |
| ------------------------------- | -------------------------- |
| Using tabs instead of spaces    | YAML parse error           |
| Inconsistent indentation        | Hierarchy broken           |
| Trailing colon with no value    | Key with `null` value      |
| Mixing list & dictionary levels | Unexpected structure error |
| Forgetting dashes in lists      | Becomes invalid format     |

---

## Cheat Sheet Summary

| Concept          | Syntax Example                    |                      |
| ---------------- | --------------------------------- | -------------------- |
| Key-Value Pair   | `key: value`                      |                      |
| List             | `- item1`                         |                      |
| Dictionary       | `key:\n  subkey: value`           |                      |
| Nested Structure | Combine lists + dictionaries      |                      |
| Multiline String | \`                                | \n  line1\n  line2\` |
| Anchor/Alias     | `&anchor`, `*alias`, `<<: *alias` |                      |
| File Separator   | `---`                             |                      |
| Comment          | `# This is a comment`             |                      |

---

## Troubleshooting YAML Files

### Online Tools

* [YAML Lint](https://www.yamllint.com/)
* [Code Beautify YAML Validator](https://codebeautify.org/yaml-validator)

### VS Code Setup

* Install "YAML Language Support by Red Hat"
* Features: syntax highlight, validation, autocompletion, linting

---

## Conclusion

YAML is essential for working with Kubernetes and Ansible. Understanding its syntax, indentation rules, and formatting can prevent frustrating configuration issues. Use online validators and good editors like **VS Code** to catch mistakes early.

---

## Additional Resources

* [Official Kubernetes YAML Docs](https://kubernetes.io/docs/concepts/configuration/overview/)
* [Ansible YAML Guide](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)

```


