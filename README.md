# Daml Fundamentals Certificate Capstone Project: Procurement Workflow App

✨ Welcome to the Daml Fundamentals Certification Capstone Procurement Workflow App! ✨

This is a backend-only capstone project that demonstrates understanding of the material covered throughout the Daml Fundamentals Certification Path. This project is a simple procurement workflow application modeling the interactions between a manufacturer and a supplier to create and fulfill purchase orders. 

Created by Anthony Janocko June 2023

---

# Procurement Workflow App
Procurement Workflow App is a backend-only procurement workflow application built in Daml.

### I. Overview 
This project was created by using the `empty-skeleton` template. The project adopts the `proposal-accept` design pattern to create a master service agreement (MSA) between two entities: a supplier and a manufacturer for a given product. An entity represents a company in a supply chain. Entities have a balance, representing currency used for payment, an inventory, representing the quantity of the product currently owned by the entity, a counterparty indicating who this entity can do business with for a particular product and a name indicating the product itself.  In this application, an entity contract represents a company's ownership of a single product. Entities can be either suppliers or manufacturers.

A manufacturer can propose a master service agreement (MSA) with a supplier with MSA Terms (a custom data type with the name of the product, an optional date and the price per good for the MSA).  A supplier can accept or reject a MSA Proposal.  A manufacturer can cancel a MSA Proposal. After supplier approval, a formal MSA is created with the MSA Terms, the two participating parties and a boolean flag indicating if the MSA is active.  Manufacturers can create Purchase Orders (POs) on active MSAs indcating a quantity of the product requested at the MSA price (POs can be created individually or in bulk by providing a list of quantities).  After the PO is created, the supplier can fulfill the PO which updates the balance and inventory of both entities depending on the terms of the MSA and the quantity of the PO. Fulfillment is dependent on both counterparties having sufficient balance and inventory for the PO.  Finally, a manufacturer can retire a MSA, rendering it inactive and preventing further POs from being created based on those terms. 


### II. Workflow
  1. manufacturer creates a MSAProposal contract     
  2. supplier exercises the MSAProposal_Accept to accept the MSAProposal and create the MSA contract
  3. manufacturer creates a PurchaseOrder contract by exercising CreatePurchaseOrder (single PO) or CreatePurchaseOrders (multiple POs)
  4. supplier exercises fulfill on a PurchaseOrder contract updating the inventory and balance of both entities
  5. manufacturer exercises RetireMSA 

### III. Challenge(s)
* This initial implementation has divulgence of the balance / inventory of counter parties in order to fulfill the PO in one transaction atomically.  This might not be feasible given privacy concerns of counter parties.  In order to address this divulgence there are two options to refactor.
  - Implement UTXO model for purchase orders and keeping the balance / inventory values in those contracts
  - Split the fulfill functionality into two contracts with a manufacturer validate choice following the propose accept pattern.  This breaks the atomicity of the fulfill function but eliminates the divulgence. 
* Other optimizations: 
  - Refactor Entity contract to have a list of counterparties to do business with.
  - Refactor to have the product of the workflow be more dynamic and support multiple products. 


### IV. Compiling & Testing
To compile and test, run the pre-written scripts indicating the section of the workflow tested and if the test is happy path or unhappy path.  The scripts can be found in the `TestPurchaseOrder.daml` file under `/daml` OR to test in the Navigator run:
```
$ daml start
```
Note: `Setup.daml` creates two entities: manufacturer and supplier.  Follow the workflow defined in section II in the Navigator to test happy path and unhappy path functionality. 