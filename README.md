# Dataverse Virtual Tables Setup Guide

## Overview
Connect MDW_Advania SQL tables to Copilot Studio via Dataverse Virtual Tables using the on-premises data gateway.

---

## Prerequisites
- [x] On-Premises Data Gateway installed and configured
- [x] SQL Server connection created in Power Platform
- [ ] Power Apps environment selected (same as Copilot Studio agent)

---

## Phase 1: Create Virtual Tables

Create in this order to enable proper relationship setup:

### Tier 1 - Core Dimension Tables (No Dependencies)
| # | SQL Table | Virtual Table Name | Primary Key | Status |
|---|-----------|-------------------|-------------|--------|
| 1 | DIM_Customer | vt_DIM_Customer | DW_Id (bigint) | ⬜ TODO |
| 2 | DIM_Department | vt_DIM_Department | DW_Id (bigint) | ⬜ TODO |
| 3 | DIM_EC_Samningar | vt_DIM_EC_Samningar | DW_Id (bigint) | ⬜ TODO |
| 4 | DIM_Resource | vt_DIM_Resource | DW_Id (bigint) | ⬜ TODO |

### Tier 2 - Dimension Tables (Link to Tier 1)
| # | SQL Table | Virtual Table Name | Primary Key | Status |
|---|-----------|-------------------|-------------|--------|
| 5 | DIM_Employee | vt_DIM_Employee | DW_Id (bigint) | ⬜ TODO |
| 6 | DIM_Contact | vt_DIM_Contact | DW_Id (bigint) | ⬜ TODO |
| 7 | DIM_Job Task | vt_DIM_Job_Task | DW_Id (bigint) | ⬜ TODO |

### Tier 3 - Dimension Tables (Link to Tier 2)
| # | SQL Table | Virtual Table Name | Primary Key | Status |
|---|-----------|-------------------|-------------|--------|
| 8 | DIM_Job Request | vt_DIM_Job_Request | DW_Id (bigint) | ⬜ TODO |
| 9 | DIM_Sales Header | vt_DIM_Sales_Header | DW_Id (bigint) | ⬜ TODO |
| 10 | DIM_Sales Order | vt_DIM_Sales_Order | DW_Id (bigint) | ⬜ TODO |

### Tier 4 - Fact Tables
| # | SQL Table | Virtual Table Name | Primary Key | Status |
|---|-----------|-------------------|-------------|--------|
| 11 | FACT_Contract Sales | vt_FACT_Contract_Sales | DW_Id (bigint) | ⬜ TODO |
| 12 | FACT_CustomerLedgerEntry_vw | vt_FACT_CustomerLedgerEntry | DW_Id (bigint) | ⬜ TODO |
| 13 | FACT_Job Journal Line | vt_FACT_Job_Journal_Line | DW_Id (bigint) | ⬜ TODO |
| 14 | FACT_Job Task | vt_FACT_Job_Task | DW_Id (bigint) | ⬜ TODO |

---

## Phase 2: Create Relationships

### Customer Relationships (DIM_Customer as Parent)
| Parent Table | Child Table | FK Column in Child | Link To in Parent | Status |
|--------------|-------------|-------------------|-------------------|--------|
| vt_DIM_Customer | vt_DIM_Contact | parentcustomerid | accountid | ⬜ TODO |
| vt_DIM_Customer | vt_DIM_Job_Request | Customer No. | Customer ID | ⬜ TODO |
| vt_DIM_Customer | vt_DIM_Job_Task | Bill-to Customer No. | Customer ID | ⬜ TODO |
| vt_DIM_Customer | vt_DIM_Sales_Header | Bill-to Customer No. | Customer ID | ⬜ TODO |
| vt_DIM_Customer | vt_DIM_Sales_Order | Bill-to Customer No. | Customer ID | ⬜ TODO |
| vt_DIM_Customer | vt_FACT_Contract_Sales | SCMH Bill-to Customer No | Customer ID | ⬜ TODO |
| vt_DIM_Customer | vt_FACT_CustomerLedgerEntry | Customer No. | Customer ID | ⬜ TODO |

### Department Relationships (DIM_Department as Parent)
| Parent Table | Child Table | FK Column in Child | Link To in Parent | Status |
|--------------|-------------|-------------------|-------------------|--------|
| vt_DIM_Department | vt_DIM_Employee | DepartmentCode | Code | ⬜ TODO |
| vt_DIM_Department | vt_FACT_Contract_Sales | keyDepartment | Code | ⬜ TODO |

### EC_Samningar Relationships (DIM_EC_Samningar as Parent)
| Parent Table | Child Table | FK Column in Child | Link To in Parent | Status |
|--------------|-------------|-------------------|-------------------|--------|
| vt_DIM_EC_Samningar | vt_DIM_Sales_Order | dimECsamningarId_vw | ECSamningarId | ⬜ TODO |
| vt_DIM_EC_Samningar | vt_FACT_Contract_Sales | dimECsamningarId_vw | ECSamningarId | ⬜ TODO |

### Job Task Relationships (DIM_Job Task as Parent)
| Parent Table | Child Table | FK Column in Child | Link To in Parent | Status |
|--------------|-------------|-------------------|-------------------|--------|
| vt_DIM_Job_Task | vt_DIM_Job_Request | Job No. | Job No. | ⬜ TODO |
| vt_DIM_Job_Task | vt_DIM_Sales_Header | Job No. | Job No. | ⬜ TODO |
| vt_DIM_Job_Task | vt_FACT_Contract_Sales | Job No. + Job Task No. | Job No. + Job Task No. | ⬜ TODO |

### Resource Relationships (DIM_Resource as Parent)
| Parent Table | Child Table | FK Column in Child | Link To in Parent | Status |
|--------------|-------------|-------------------|-------------------|--------|
| vt_DIM_Resource | vt_FACT_Contract_Sales | keyResourceNo | No. | ⬜ TODO |

---

## Phase 3: Connect to Copilot Studio

| Step | Action | Status |
|------|--------|--------|
| 1 | Open Copilot Studio | ⬜ TODO |
| 2 | Go to Agent → Knowledge → Add Knowledge | ⬜ TODO |
| 3 | Select Dataverse | ⬜ TODO |
| 4 | Add virtual tables as knowledge sources | ⬜ TODO |
| 5 | Test queries | ⬜ TODO |

---

## Step-by-Step: Creating a Virtual Table

1. Go to [make.powerapps.com](https://make.powerapps.com)
2. Select correct environment
3. Go to **Solutions** → Open/create solution
4. Click **New** → **Table** → **Virtual table**
5. Select **SQL Server**
6. Choose your gateway connection:
   - Server: `[your server]`
   - Database: `MDW_Advania`
   - Auth: SQL Server / Windows
   - Gateway: `[your gateway name]`
7. Click **Create**
8. Search for table (e.g., `DIM_Customer`)
9. Configure names and click **Finish**

---

## Step-by-Step: Creating a Relationship

1. Go to **Tables** → Select child table
2. Click **Relationships** in left menu
3. Click **+ New relationship** → **Many-to-one**
4. Select parent virtual table
5. Map the lookup column (FK field)
6. Save

---

## Testing Notes

### Test 1: Unlinked Tables (Before Relationships)
- [ ] Test date: ___________
- [ ] Tables tested: ___________
- [ ] Agent response quality: ___________
- [ ] Notes: ___________

### Test 2: Linked Tables (After Relationships)
- [ ] Test date: ___________
- [ ] Tables tested: ___________
- [ ] Agent response quality: ___________
- [ ] Notes: ___________

---

## Known Limitations

- ⚠️ Cannot filter/sort by virtual table lookup columns in views
- ⚠️ Lookups between virtual tables can be slower
- ⚠️ Search functionality not supported for virtual tables
- ⚠️ Charts/dashboards not supported for virtual tables

---

## Architecture Diagram

```
                    ┌─────────────────┐
                    │  DIM_Customer   │
                    └────────┬────────┘
           ┌─────────────────┼─────────────────┬──────────────┐
           ▼                 ▼                 ▼              ▼
    ┌─────────────┐  ┌──────────────┐  ┌─────────────┐  ┌──────────────────────┐
    │ DIM_Contact │  │ DIM_Job Task │  │ DIM_Sales   │  │ FACT_CustomerLedger  │
    └─────────────┘  └──────┬───────┘  │   Header    │  │     Entry_vw         │
                            │          │   Order     │  └──────────────────────┘
                            ▼          └──────┬──────┘
                    ┌──────────────┐           │
                    │DIM_Job Request│          │
                    └──────────────┘           ▼
                                        ┌──────────────────┐
    ┌─────────────────┐                 │ FACT_Contract    │
    │ DIM_Department  │────────────────▶│     Sales        │◀─── DIM_Resource
    └────────┬────────┘                 └────────┬─────────┘
             │                                   │
             ▼                                   │
    ┌─────────────────┐                          │
    │  DIM_Employee   │                          │
    └─────────────────┘                          │
                                                 │
    ┌─────────────────┐                          │
    │ DIM_EC_Samningar│──────────────────────────┘
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │ DIM_Sales Order │
    └─────────────────┘
```

---

## Changelog
| Date | Change |
|------|--------|
| 2026-02-26 | Initial setup guide created |
 
