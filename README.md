# Hi, I'm Leonardo 👋

Senior Backend Engineer with 15+ years of experience specializing in backend application development, application modernization, and legacy code refactoring. Proven track record of designing modular architectures, RESTful APIs, and scalable backend solutions.

## <img src="doc/images/rivendel.svg" alt="Rivendel logo" /> Conciliaciones (Rivendel)

System for managing labor claims and issuing conciliation agreements.

A full-stack web app in production, built end to end as a solo developer.

- Google Calendar integration via OAuth 2.0, syncing each conciliator's hearings to their own calendar
- Dynamic PDF generation of conciliation records, adapted to the resolution type and the parties involved
- Rich-text settlement documents built with Tiptap
- Cross-domain authentication with short-lived access tokens and rotating refresh tokens
- Third-party bank integration for account verification, behind anti-bot protection

[Live demo](https://conciliaciones-demo.vercel.app/) · [Frontend repository](https://github.com/leoariaspaz/rivendel-client) · [Backend repository](https://github.com/leoariaspaz/rivendel) · [Case study](./doc/case-studies/conciliaciones.md)

## <img src="doc/images/ismp.jpg" alt="ISMP" width="20"> SMPorres — Tuition Collections System

A collections system I co-developed with a friend for a technical-career institute in Argentina, replacing a legacy Access database and a manual reconciliation process. It covers the full cycle: enrolling students, generating payment plans, issuing bank payment slips with barcodes, importing the bank's settlement file to register payments, and notifying debtors by email, plus a web portal where students check their own account.

- Real banking integration: barcode generation with a check digit and automatic reconciliation of the bank's settlement files.
- Non-trivial business rules: scholarships, early-payment discounts and late fees computed in tranches by rate validity period, applied consistently by the desktop app and the web portal.
- Several delivery channels over one domain: Windows Forms back office, ASP.NET MVC portal, REST API with JWT, and scheduled jobs for debt notices and Google Drive backups.

[Repository](https://github.com/leoariaspaz/SMPorres) · [Case study](./doc/case-studies/ismp.md)

## 📫 Get in touch

- [LinkedIn](https://www.linkedin.com/in/leonardo-arias-paz/)