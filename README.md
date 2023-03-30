# Indexing

Step 1: Download data from Below Link :

https://nitincolortoken.s3.ap-south-1.amazonaws.com/Archive.zip

Step 2 : Load data via compass to MongoDB

Step 3 : Run below queries on people collection

======= Single Field Indexes =================

{ "ssn" : "720-38-5636" }

db.people.createIndex( { ssn : 1 } )

{ ssn : { $gte : "555-00-0000", $lt : "556-00-0000" } }

{ "ssn" : { $in : [ "001-29-9184", "177-45-0950", "265-67-9973" ] } }

{ "ssn" : { $in : [ "001-29-9184", "177-45-0950", "265-67-9973" ] }, last_name : { $gte : "H" } }

Sorting Example:

In Memory - SORT Stage

Using Index

db.people.find({}, { _id : 0, last_name: 1, first_name: 1, ssn: 1 }).sort({ ssn: 1 }).explain("executionStats")

db.people.find({}, { _id : 0, last_name: 1, first_name: 1, ssn: 1 }).sort({ first_name: 1 }).explain("executionStats")


======= Compound =================

{ "last_name": "Frazier", "first_name" : "Jasmine” }

create index : last name 

create compound index : (last name and first name)

Nw lets see explain of below query

{ "last_name": "Frazier", "first_name" : {$gte : "L"} }

======= Index Prefix =================

Run below queries

{ “last_name” : “Solomon” }

{ “first_name” : “Sonia” }


Create below Index :

job , employer , last name , first name

Run below queries :

{ job : “Jewellery Designer” , employer : “Baldwin-Nichols”}

{ job : “Jewellery Designer” , employer : “Baldwin-Nichols”, last_name: "Cook"}

{ job : “Jewellery Designer” , employer : “Baldwin-Nichols”, first_name: "Sara"}

{ job : “Jewellery Designer” , first_name: "Sara", last_name: "Cook"}


===================  ESR Example for sample_mflix movies collection ==========================


query = { amenities: "Waterfront",
  "bed_type" : { $in : [ "Futon", "Real Bed" ] },
  first_review : { $lt: ISODate("2018-12-31") },
  last_review : { $gt : ISODate("2019-02-28") } 
}
project = { bedrooms:1 , price: 1, _id:0, "address.country":1}
order = {bedrooms:-1,price:1}
db.listingsAndReviews.find(query,project).sort(order)

amenities_1_bedrooms_-1_price_1_bed_type_1_first_review_1_last_review_1

