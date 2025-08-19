# Signum Token-Service Design Repository

<p align="center">
    <img height="600" src="./diagrams/logo/Signum Token Service Logo.png" alt="Signum Token Service Logo"/>
</p>

---

📘 **Central source for architecture and API design docs for Signum Token Service**

---

## 🧩 What Is This Repository?

This repository contains all **design artifacts** related to the **Signum Token Service**, which handles **Token generation, signing, and validation**. It includes:

* High-level architecture, sequence and component diagrams
* Token specifications and security flows
* Data models, design ADRs, and API contract definitions
* Start looking here: [System Design Document](./system.design.md)

---

## 📂 Folder Structure

```text
/
├── diagrams/                                                 
|   └── logo/                                    
│       ├── Signum Token Service Logo.jpeg
├── references/                                   
|   └── research.md
├── specs/                                   
│   ├── id-spec.yaml                                 
|   └── token-spec.yaml
├── CHANGELOG.md                                  
├── functional-requirements.md                     
├── LICENSE                        
├── README.md                               
└── system-design.md                                 
```

---

## ✅ Getting Started

1. **Clone the Repo**

   ```bash
   git clone https://github.com/Phylax-IAM/Signum.Token-Service.Design.git
   ```

2. **Explore the Design Docs**

   * Diagrams in `/diagrams/img/`
   * Logos in `/diagrams/logo/`

3. **Your Feedback & Contributions**

   * Hit the Issues tab or open a PR

---

## 📋 Key Documents Explained

* **`system-design.md`** – Serves as the primary reference for all high-level architectural decisions, system context, and component interactions within the Phylax IAM platform.
  
* **`functional-requirements.md`** – Lists and explains the core functional requirements of the API Gateway MVP, including routing, rate limiting, caching, and more.

---

## 🛠 How to Use These Docs

* As a **reference for frontend/backend implementation**
* For **auto-generating mock servers or tests** from OpenAPI spec
* To maintain **naming/response consistency across phases**
* As source-of-truth during architecture reviews or onboarding

---

## 🤝 Contributing

Please follow these conventions:

* Write clear and consistent API/field names
* Add diagrams or descriptions when changing or introducing new endpoints
* Use [Conventional Commit Messages](https://www.conventionalcommits.org/en/v1.0.0/)

---

## 📄 License

[MIT License](./LICENSE) – See LICENSE file for details.

---

## 🔗 Contacts

For questions or feedback, reach out to:

* **Authored by**: *Pragyanshu Rai* (`<pragyanshu.rai.phylax.iam@gmail.com>`)
* **Project Lead**: **Pragyanshu Rai**
