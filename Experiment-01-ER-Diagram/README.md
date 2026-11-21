# Experiment 1 - ER Diagram for an Indian E-commerce Platform

---

## Student Details

| Name | UID No. | Course | Subject | Submitted To |
|:---|:---|:---|:---|:---|
| Pragya Dubey | 25LBCS3110 | B.Tech CSE (AI/ML-3) | RDBMS (25CSH-210) | Mr. Amit Kumar Sir,   Mr. Ilapanda Pavan Kumar Sir |

---

## Aim

Design an Entity-Relationship (ER) diagram for an Indian e-commerce platform with the entities **Customer, Product, Order, OrderItem, Seller, Category, Payment, Delivery and Address**. Show the primary keys, composite and multi-valued attributes, weak entities, the specialization of Product, and participation constraints.

---

## Software / Tools Used

| Tool | Purpose |
|:---|:---|
| Graphviz (dot, neato layout) | Drawing the ER diagram from a text description (`er_diagram.dot`) |
| VS Code | Editing the diagram source |
| GitHub (Mermaid) | Second view of the same model in Crow's Foot notation |

---

## Theory

**ER Model:** The Entity-Relationship model (P. P. Chen, 1976) describes a database at the conceptual level as entities, their attributes, and the relationships between them.

### ER Concepts and Chen Notation

| Concept | Meaning | Chen Notation |
|:---|:---|:---|
| **Strong Entity** | Has its own primary key | Rectangle |
| **Weak Entity** | Cannot be identified by its own attributes; depends on an owner entity | Double Rectangle |
| **Identifying Relationship** | Links a weak entity to its owner | Double Diamond |
| **Key Attribute** | Uniquely identifies an entity | Underlined Ellipse |
| **Partial Key (Discriminator)** | Identifies a weak entity within one owner | Dashed Ellipse / Dashed Underline |
| **Composite Attribute** | Can be divided into sub-parts | Ellipse connected to sub-ellipses |
| **Multi-valued Attribute** | Can hold many values for one entity | Double Ellipse |
| **Derived Attribute** | Computed from other attributes | Dashed Ellipse |
| **Total Participation** | Every entity must take part in the relationship | Double Line |
| **Partial Participation** | Taking part is optional | Single Line |
| **Cardinality Ratio** | 1:1, 1:N, M:N | Labels on the edges |
| **Specialization / ISA** | Sub-classes of a super-class entity | Triangle; d = disjoint, o = overlapping |

---

## ER Diagram



## Design Explanation

### 1. Entities and Primary Keys

| Entity | Type | Primary Key |
|:---|:---|:---|
| Customer | Strong | customer_id |
| Seller | Strong | seller_id |
| Category | Strong | category_id |
| Product | Strong (Super-class) | product_id |
| Order | Strong | order_id |
| Payment | Strong | payment_id |
| Delivery | Strong | delivery_id |
| Address | Weak (Owner: Customer) | {customer_id, address_no} |
| Order_Item | Weak (Owner: Order) | {order_id, line_no} |
| Electronics | Sub-class of Product | product_id (inherited) |
| Clothing | Sub-class of Product | product_id (inherited) |
| Book | Sub-class of Product | product_id (inherited) |

**Note:** `address_no` and `line_no` are the partial keys (discriminators) of their respective weak entities.

### 2. Special Attributes

| Kind | Attribute(s) | Why |
|:---|:---|:---|
| Composite | Customer.name → first_name, middle_name, last_name | Indian names often have an optional middle name |
| Multi-valued | Customer.phone_no, Seller.contact_no, Book.author | A customer can have several mobile numbers; a book can have several authors |
| Derived | Customer.age (from dob), Order.total_amount (from order items) | Computed, not stored, to avoid inconsistency |
| Unique (Candidate Keys) | Customer.email, Seller.gstin, Seller.pan, Payment.transaction_ref, Delivery.awb_no | Business identifiers |

### 3. Weak Entities

**Address**

"Address #1 of customer 17" only makes sense together with the customer.

- Owner Entity: Customer
- Partial Key: address_no
- Composite Primary Key: {customer_id, address_no}
- Identifying Relationship: HAS
- If the customer is deleted, their addresses go too.

**Order_Item**

"Line 2 of order 1005" depends on the order.

- Owner Entity: Order
- Partial Key: line_no
- Composite Primary Key: {order_id, line_no}
- Identifying Relationship: CONTAINS

### 4. Specialization of Product

Product is specialized into three sub-classes:

| Super-class | Sub-class | Special Attributes |
|:---|:---|:---|
| Product | Electronics | warranty_months, power_rating |
| Product | Clothing | size, fabric, gender |
| Product | Book | isbn, author, language |

**Specialization Constraints:**

- **Disjoint (d):** A product belongs to at most one sub-class.
- **Partial:** Some products (e.g. groceries) belong to no sub-class.

### 5. Relationships, Cardinality and Participation

| Relationship | Entities | Cardinality | Participation |
|:---|:---|:---|:---|
| HAS (identifying) | Customer - Address | 1:N | Address Total, Customer Partial |
| PLACES | Customer - Order | 1:N | Order Total, Customer Partial |
| SHIPS_TO | Order - Address | N:1 | Order Total |
| CONTAINS (identifying) | Order - Order_Item | 1:N | Total on both sides |
| FOR | Order_Item - Product | N:1 | Order_Item Total |
| SELLS | Seller - Product | 1:N | Product Total |
| BELONGS_TO | Product - Category | N:1 | Product Total |
| SUBCATEGORY_OF (recursive) | Category - Category | 1:N | Partial |
| PAID_BY | Order - Payment | 1:N | Payment Total |
| SHIPPED_VIA | Order - Delivery | 1:1 | Delivery Total, Order Partial |

---

## Procedure

1. Listed the requirements of an Indian e-commerce site: customers with several addresses and phone numbers, GST-registered sellers, a category tree, UPI/COD payments, and courier deliveries.
2. Identified the entities and chose a primary key for each one.
3. Classified the attributes as simple, composite, multi-valued or derived.
4. Identified the weak entities (Address, Order_Item) and their partial keys.
5. Identified the relationships and decided the cardinality and participation of each.
6. Modelled the Product sub-classes with a disjoint, partial ISA hierarchy.
7. Wrote the diagram in Graphviz DOT language and rendered it to PNG and SVG.

---

## Files

| File | Description |
|:---|:---|
| `README.md` | Experiment documentation |
| `er_diagram.dot` | Source code of the diagram (Graphviz) |
| `er_diagram.png` | ER diagram (image) |
| `er_diagram.svg` | Scalable vector version of the ER diagram |

---

## Result

The ER diagram for the e-commerce platform was successfully created. It contains:

- 9 main entities.
- 2 weak entities (Address and Order_Item).
- Primary keys and partial keys.
- Composite, multi-valued and derived attributes.
- ISA specialization of Product.
- Cardinality and participation constraints for every relationship.

---

## Conclusion

Making the ER diagram first helped me understand the data and the rules before writing any table. The weak entities, multi-valued attributes and participation constraints shown here decide the extra tables, keys and NOT NULL constraints in the next experiment.

---

<div align="center">

**END OF EXPERIMENT 1**

**RDBMS LAB | B.TECH CSE (AI/ML-3)**

</div>
