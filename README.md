# Creator Platform V1  
End-to-end creator project platform with a $1 entry point, full Framer UI, Outseta authentication, Stripe billing, structured onboarding, song-level workspaces, rights metadata intake, project progress tracking, and enterprise-ready documentation.

---

## Overview
Creator Platform V1 is a complete workflow system for emerging artists, producers, and songwriters. Users enter through a $1 beta flow, authenticate with Outseta, and onboard through a structured 3-step setup. Once inside, creators manage a 3-song project via a full portal that includes audio uploads, lyrics management, rights/splits intake, technical metadata, and export workflows.

This repository contains:
- ASCII UI wireframes of every screen  
- Data models and JSON schemas  
- Framer component structure  
- Outseta + Stripe workflow notes  
- Information architecture and onboarding flows  
- Project-level, track-level, and rights-level definitions

The goal is to provide a crystal-clear, implementation-ready system for product teams or contributors.

---

## Core Features

### **1. $1 Entry Funnel**
- Public landing page  
- Pricing page  
- $1 Stripe checkout  
- Account creation via Outseta  
- Immediate routing into onboarding  

### **2. Authentication**
- Full Outseta login widget  
- Email/password sign-in  
- Password reset  
- Private creator portal access  

### **3. Onboarding Engine (3 Steps)**
- **Step 1: Creator Profile**  
- **Step 2: Project Basics**  
- **Step 3: Song Shells (3 required)**  

Each step includes validated data entry and progress persistence.

### **4. Creator Home**
- “Mission Control” for active projects  
- Project progress bar  
- Library of past projects  
- Quick links to continue onboarding or edit songs  

### **5. Project Console**
- Project overview  
- Metadata summary  
- Song list with status  
- Next-best-action guidance  
- Workflow indicators  

### **6. Song Workspace**
- **Audio section** (upload/replace/stems optional)  
- **Credits & Technical Metadata**  
- **Lyrics module**  
- **Rights & Splits summary**  
- Structured export prep  

### **7. Data & Rights Architecture**
- Songwriter/producer attribution  
- Master ownership  
- Publishing metadata  
- Split summaries  
- Export-ready documentation (PDF/CSV future support)  

### **8. Framer CMS Architecture**
This repo includes recommended CMS collections for:
- Creators  
- Projects  
- Songs  
- Rights metadata  
- Genres  
- Intended use cases  
- Song types  

---

## Screens Included (ASCII UI Wireframes)
All major screens are represented via clean ASCII diagrams for rapid development:

1. Public Landing  
2. Pricing / $1 Offer  
3. Login  
4. Signup & Payment  
5. Onboarding Step 1  
6. Onboarding Step 2  
7. Onboarding Step 3  
8. Creator Home  
9. Project Console  
10. Song Workspace  

Full ASCII wireframes are located in `/docs/ascii-wireframes/`.

---

## Tech Stack

### **Frontend**
- Framer (pages, components, CMS data)  
- Framer interactions  
- Custom components for forms and intake  

### **Backend / Identity**
- Outseta (authentication + membership)  
- Stripe (payment)  

### **Content Layer**
- Framer CMS (collections, fields, relationships)  
- Optional static JSON for local prototyping  

---

## Repository Structure

```plaintext
/
├── docs/
│   ├── ascii-wireframes/
│   ├── data-models/
│   ├── cms-schema/
│   └── architecture/
├── src/
│   ├── framer/
│   ├── components/
│   └── assets/
├── README.md
├── .gitignore
└── LICENSE
