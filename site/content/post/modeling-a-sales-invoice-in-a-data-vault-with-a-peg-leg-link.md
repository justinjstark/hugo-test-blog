---
title: Modeling a Sales Invoice in a Data Vault with a Peg Leg Link
draft: true
date: '2016-07-06T00:00:00-05:00'
---
One of the great things about Data Vault modeling is its simplicity. There are only three types of entities: Hubs, Links, and Satellites. Hubs are the business entities. Links join any combination of Hubs and other Links. And Satellites hang from Links and Hubs to hold the temporal quantifiers and descriptors.

As with any method of modeling, however, there are multiple ways to represent the information. It is our job, as developers, to work with the business owners and determine what model makes the most sense while meeting the business requirements. And sometimes we run into cases where our business model doesn’t fit our modeling constraints and we need to bend the rules.

A common business model is a Sales Invoice and its associated Line Items. The following model, represented in Third Normal Form, allows for one Product per Line Item and one Customer per Sales Invoice. We will work on modeling this relationship in a Data Vault.

![The Sales Invoice model in Third Normal Form (3NF)](/img/SalesInvoice3NF1.svg)

Let’s start small and concentrate on modeling the relationship between a Sales Invoice and its Line Items. Then we’ll add Customer and Product back in. This is our starting model in Third Normal Form.

![The Sales Invoice to Line Item relationship in Third Normal Form (3NF)](/img/SalesInvoice3NF2.svg)

The first step in building a Data Vault model is identifying the Hubs. SalesInvoice is a business entity that can stand on its own. It is a Hub.

But what about LineItem? We might think of a Line Item as a unit of purchase. Consider if LineItem, instead of being identified by SalesInvoiceID and LineNumber, was represented by a unique identifier of its own. Then we might convince ourselves that LineItem is a Hub and build such a Data Vault model.

![The Sales Invoice to Line Item relationship in a Data Vault with Line Item as a Hub](/img/SalesInvoiceDV1.svg)

This works, but is it the best way to model the data?

One indication that we can do better is the Link table between HubSalesInvoice and HubLineItem is difficult to name. What do you call the relationship between the Sales Invoice and the Line Item? When you have trouble naming your tables, it is a good indicator that the model needs improved.

In our case, the difficulty in naming the Link table is a symptom of redundant modeling. In HubLineItem, we have a relationship between SalesInvoiceID and LineNumber stored as natural Business IDs. In LnkLineItems, we have the same relationship stored as surrogate IDs.

We’ve also given a clue by identifying the relationship between LnkLineItem and HubLineItem as one-to-one instead of the standard one-to-many relationship between a Hub and a Link. There is no need for both of these tables to exist. By combining them, we can simplify the model, remove the redundancy, and improve the performance of the warehouse load.

But how can we combine a Hub and a Link in a Data Vault? This is where we need to bend the rules and create what’s known as a Peg Leg Link.

![The Sales Invoice to Line Item relationship in a Data Vault with Line Item as a Peg Leg Link](/img/SalesInvoiceDV2.svg)

LnkLineItem is a Peg Leg Link because it does not link any two Data Vault elements together. Instead, it connects a Sales Invoice to a Line Number which is not a real business entity and has no business being a Hub. It does not matter to our data warehouse what values exist in LineNumber, only that they are unique to a Sales Order in order to group purchased items. These groups may be based on purchase price (think buy one get one half off) and any number of other factors. So, a Line Item is really a link between all of these things, grouped by a Line Number.

Our model with a Peg Leg Link makes sense, so we are ready to add Product back in to our Data Vault model.

![The Sales Invoice Model in a Data Vault with Product included](/img/SalesInvoiceDV3.svg)

Again, this works but it can be improved. Notice that LnkProductPurchased is a Link on a Link. While this does not violate most teams’ rules for Data Vaults, whenever you see a Link connected to another Link, it is often best to simplify the model by combining the Links.

![The Sales Invoice Model in a Data Vault with a single Link](/img/SalesInvoiceDV4.svg)

Note that the unique constraint on LnkLineItem should still be the composite HubSalesInvoiceSID and LineNumber, however, we also have the relationship to Product embedded in the Link.

Finally, we can add Customer back in to our Data Vault model.

![The Sales Invoice Model in a Data Vault with Customer included](/img/SalesInvoiceDV5.svg)

This is about as simple as our Data Vault model can get. While the model has over twice as many tables as the original Third Normal Form model, we have managed to keep the complexity to a minimum by evaluating the model and removing redundancies along the way.
