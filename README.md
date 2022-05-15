# Database Module - Exam 

## Preface 

The main aim is to automate the management system of a car parts warehouse. 
The specifications of the system, acquired through interview, are those below reported.
Analyze these specifications, filter the ambiguities present and then group them in a homogeneous way.
Identify the links between the various groups of specifications thus obtained.
Realize the design of the conceptual model and represent the specifications (after the phase of
reorganization) with a schema of the Entity-Relationship model. Carry out the logical design
of the database of the information system and its implementation both in the relational model
than in a non relational model based on document management.

## Specifications
Consider a database that contains information about the purchases of customers of a store of
car parts.
Customers are interested in the tax code or VAT number, which identifies them, the name, the address of residence
complete with zip code, city, province and region.
Of each expense of a customer concern the number of the invoice, which identifies it, the date, the total of
shopping, the method of payment (electronic, cash) and, for each product, the quantity, the price
paid and any discount applied (product on promotion).
Of each part concerned the code, which identifies it, the description, the category, the vehicles
compatible, the unit cost and the selling price. Products may be affected by
promotions, with temporary price reduction, from a certain date and for a number
set of days.

## Required SQL queries 

1. For a specific customer, determine the quantities of each product purchased in a
given period;
2. For a given product, determine the number of distinct customers who have
purchased in a given period;
3. Identify all customers who have purchased a promotional product, indicating the
Postal code of residence;
4. For a specific CAP, identify customers who have made purchases in a particular CAP
period;
5. For a specific car model, identify the turnover of the items in a given
period;
6. For each promotion item, determine the quantity sold in a given item
period;
7. Customers who have not made an electronic payment in the current year;
8. The customer who has spent more in a given period;
9. For a specific postal code, the average expenditure per invoice over a given period;
10. For each category of product, the total price paid for those sold in a
given period.

## Analysis 

### Assumptions
Given the current database the following assumptions are made: 

-  It's assumed that all Italian Regions, Provinces, Cities and Postcodes are present in the database
-  Every customer in the database purchased at least once at the car parts warehouse
-  Each product belongs to a single category
-  Each replacement is compatible with one or more vehicles
-  The invoice number is equivalent to the receipt of purchase
-  One or more products belong to a promotion
-  A product may have 0 or N promotions
-  If the Brand is present has at least one vehicle in the database

## Key-points related to the main entities: 
-	Key-points related to Customer: For each customer it’s known about name, surname, identifiy code and residence address. A customer could be a Company or a Private Customer.
-	Key-points related to Order: For each order that is placed it’s known a identify code and the data where order has been placed. Each order contains one or more products.
-	Key-points related to Vehicle: For each vehicle it’s known a identify code and the name vehicle. Each vehicle is producted by a Brand that we identify with a ID code and the brand name. 
-	Key-points related to Product: Each product is marked with a identify code, the name, a short description, the selling price, the unit cost price. A product could be on sale or not. If the product is on sale it’s known a identifier code that mark the product on sale, the percentage off, and the data where promotion starts and ends. 
-	Key-points related to Payment: When an order is placed the customer does the payment. The payment could be with cash or electronic. For the last one we know also the identifier transaction code.

# Glossary Terms
|     **Terms**    |     **Synonymous**                                                                      |     **LinkedTo**                    |     **Description**                                                                                                            |
|------------------|-----------------------------------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
|     Customer     |     User/Buyer                                                                          |     Shopping,   Product             |     A person that   buys on a shopping session.                                                                                |
|     Expense      |     /                                                                                   |     Customer,   Product             |     Total   purchases during a shopping session made by a Customer                                                             |
|     Product      |             /                                                                           |     Expense,   Product              |     Product is   contained in Expense. It could be buy in quantities that Customer needs, and   it could be on sale or not.    |
|     Promotion    |     Reduction of   Price Cost                                                           |     Product                         |     A special percentage   off on a price product. This condition could interest one or more product or   neither.             |
|     Payment      |             /                                                                           |     Expense,   Product, Customer    |     An order   could be pay using more than a Payment Type. Cash or electronic payment.                                        |
| Category         | A category standardizes a group of products that share similar qualities or functions.  |     /                               | Product                                                                                                                        |
# ER Model 
## First Version 

<a href="https://ibb.co/G9FxBbM"><img src="https://i.ibb.co/f9Q1BPN/nonristrutturato.png" alt="nonristrutturato" border="0"></a>

On the first version we can find different entities:

- **Customer**: this entity represents a person who buy prodcuts at the special parts shop. The Customer has some data that we use to identify himself. The `id_code` is the unique code client. We know also the _Residence Address_ and general informations as name and surname. The entity presents the specification _Company_ or _PrivateCustomer_ useful to understand the type of Customer. Based on the result the Customer will be associated on the attribute _VAT_ or _id_code_. 

A Customer could place one or more order. 

- **Order**: the entity _order_ represents the purchase that could contains at least one or more products. Each order got the idenitifier code, _id_code_, and the data where is placed. 

A order belongs to a single customer. 

- **Payment**: the entity _payment_ is consequential to the order that is placed. It contains the attributes `id_payment`, _TotalSpending_ and the specializations _Cash_ and _ElectronicPayment_, marked with a `id_transaction`.

An order could have only a type of payment. 

- **Product**: the entity product identifies all the products available in database and ready to be sold. Each _Product_ has a `id_product`, name, short description, selling price and the unit cost. The entity has also a specialization _OnSale_ and _NotOnSale_. _OnSale_ contains `id_sale` to mark the single product that it's on promotion, the percentage off and the data where start and finish. 

A product could be contained in one or more order and the promotion is a optional condition. 

- **Category**: This entity identifies the categories where products belong to due to similar qualities or functions. The entity has `id_cat_prod`and a name. 

One product belongs on a unique category, but a category could contains one or more products. 

- **Vehicle**: This entity is strictly linked to the entity _Product_ because represents all the vehicles that are present on the database that could be compatible with the single special parts. This entity has `id_vehicle`and the attribute _name_vehicle_ to nominate all the actors. 

One or more products could be compatible with a Vehicle or more, and a Vehicle could be compatible with one or more products too. 

- **Brand**: This entity identifies the Brands that produce the vehicles on the database that are potentially compatible with the special parts on selling. Brand has `id_brand` and the attribute _name_brand_. 

A vehicle could be produced by only a Brand, but a Brand that is present in the database could produce at least one wehicle ore more. 

# ER Model 
## Second Version - Restructured and normalized 

<a href="https://ibb.co/kx2gGwX"><img src="https://i.ibb.co/KXxj5fW/ristrutturato.png" alt="ristrutturato" border="0"></a>

On the second version we got the following entities: 

- **CAP**: Identifies all the postal codes that belong to the same location. We ca identify it with the `id_cap`. A CAP belongs to only a city. A Customer resides in a specifical CAP. 

- **City**: Identifies all the italian cities that have a CAP and are located in a Province. A city may belongs to more than one CAP, but it could be located in one and only one Province. We identify a City with `id_city`. 

- **Province**: Identifies where a City is located and it's detected with `id_province`. 

- **Region**: Identifies where a Province is located. A Region, detected with the `id_region`could contains more than a Province, but a Province could be situated on only a Region. 

- **Customer**: Identifies one or more person that buy at the special parts shop. The Customer has some data that we use to identify himself. The `id_code` is the unique code client. We know also the _Residence Address_, _Name_, _Surname_, _CustomerType_, `fiscal_code`, `VAT`and the `id_customer`. 

A customer could place one or more order. 
A customer could make one or more payments. 

- **Order**: the entity _order_ represents the purchase that could contains at least one or more products. Each order got the idenitifier code, _id_code_, and the data where is placed. 

Order includes one or more products. 
Order could be sale with a unique Payment. 
Order belongs to only and only one Customer. 

- **Payment**: the entity _Payment_ is consequential to the order that is placed. It contains the attributes `id_payment`, _PaymentType_ and _id_transaction_. 

- **Product**: the entity product identifies all the products available in database and ready to be sold. Each _Product_ has a `id_product`, name, short description, selling price and the unit cost. 

- **OnSale**: the entity _OnSale_ represents all the products that are on sale during a defined time. The identifier is `id_promo`and the attributes are _PercentOff_, _DataStart_ and _DataFinish_. 

- **Vehicle**: This entity is strictly linked to the entity _Product_ because represents all the vehicles that are present on the database that could be compatible with the single special parts. This entity has `id_vehicle`and the attribute _name_vehicle_ to nominate all the actors. 

One or more products could be compatible with a Vehicle or more, and a Vehicle could be compatible with one or more products too. 

- **Brand**: This entity identifies the Brands that produce the vehicles on the database that are potentially compatible with the special parts on selling. Brand has `id_brand` and the attribute _name_brand_. 

A vehicle could be produced by only a Brand, but a Brand that is present in the database could produce at least one wehicle ore more. 

# Volume Table 

| Element  | Type   | Volume |
|----------|--------|--------|
| Customer | Entity | 1000   |
| Order    | Entity | 3000   |
| Product  | Entity | 50.000 |
| Vehicle  | Entity | 5000   |
| Brand    | Entity | 150    |
| CAP      | Entity | 41     |
| City     | Entity | 7915   |
| Province | Entity | 107    |
| Region   | Entity | 20     |

# Logic Model 

- **Customer** (<ins>ID_Customer</ins>, Name, Surname, CustomerType, Fiscal_Code, VAT)

- **Order** (<ins>ID_Order</ins>, Data, _Customer, _Payment_)

- **OrderContainsProduct** (_ID_Customer_, _ID_Product_, Quantity) 

- **Product** (<ins>ID_Product</ins>, Name, Description, UnitCost, SellingPrice)

- **Category_Product** (<ins>ID_Cat_Prod</ins>, Name_Cat_Prod, _ID_Product_) 

- **OnSale** (<ins>ID_Promo</ins>, PercentOff, DataStart, Data Finish, _Products_) 

- **Vehicle** (<ins>ID_Order</ins>, Name_Vehicle, _ID_Brand_)

- **ProductCompatibleVehicle** (_ID_Product_, _ID_Vehicle_) 

- **Brand** (<ins>ID_Brand</ins>, Name_Brand)

- **Payment** (<ins>ID_Payment</ins>, PaymentType, Tipo)

- **CAP** (<ins>Cap_Code</ins>, Name, _City_) 

- **City** (<ins>ID_City</ins>, Name, _Province_)

- **Province** (<ins>ID_Province</ins>, Name, _Region_) 

- **Region** (<ins>ID_Region</ins>, Name)
