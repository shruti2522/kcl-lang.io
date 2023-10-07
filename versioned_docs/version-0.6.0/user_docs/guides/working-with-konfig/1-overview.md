---
id: overview
sidebar_label: Overview
---
# Konfig Overview

In KCL, it is recommended to uniformly manage all configurations and model libraries in the way of **configuration library**, that is, to store not only KCL definitions of the abstract model itself, but also various types of configurations, such as application operation and maintenance configuration, policy, etc. The configuration is recommended to be hosted in various VCS systems to facilitate configuration rollback and drift check. The best practice code of the configuration repository is Konfig, and the repository is hosted in [Github](https://github.com/kcl-lang/konfig)。

⚡ The Konfig repository mainly includes:

* KCL module declaration file (kcl.mod)
* KCL domain model libraries (Kubernetes, Prometheus, etc.)
* Directories of various configurations (application operation and maintenance configuration, etc)
* Configuration build and test scripts (Makefile, Github CI file, etc.)

The reason for using a unified warehouse to manage all KCL configuration codes is that different code packages have different R&D entities, which will lead to package management and version management problems. When the business configuration code and basic configuration code are stored in a unified warehouse, the version dependency management between codes will be relatively simple. By locating the directory and file of the unique code base, the configuration code can be managed uniformly for easy search, modification and maintenance.

The following is the architecture of Konfig:

![](/img/docs/user_docs/guides/konfig/konfig-arch.png)

Konfig provides users with an out-of-the-box and highly abstract configuration interface. The original simple starting point of the model library is to improve the efficiency and experience of Kubernetes YAML users. We hope to simplify the writing of user-side configuration code by abstracting and encapsulating the model with more complex code into a unified model. Konfig consists of the following parts:

* **Core model**:
  * **Front-end model**: The front-end model is the "user interface", which contains all configurable attributes exposed to users on the platform side. Some repetitive and deducible configurations are omitted, and essential attributes are abstracted and exposed to users. It has user-friendly features, such as `server.k`.
  * **Back-end model**: The back-end model is "model implementation", which is the model that makes the properties of the front-end model effective. It mainly contains the rendering logic of the front-end model instance. The back-end model can use KCL to write validation, logic judgment, code fragment reuse and other code to improve the reusability and robustness of the configuration code, and is not sensitive to users, such as `server_backend.k`.
* **Domain model**: It is a model that does not contain any implementation logic and abstraction. It is often generated by tool transformation and does not need to be modified. It corresponds to the real effective YAML attribute one by one. The domain model needs to be further abstracted and is generally not directly used by users. For example, `kusion_kubernetes` is the domain model library of Kubernetes scenarios.

In addition, the core model simplifies the configuration code of front-end users through two layers of abstraction: the front-end model and the back-end model. The domain model is automatically generated through the [KCL OpenAPI tool](/docs/tools/cli/openapi/quick-start).