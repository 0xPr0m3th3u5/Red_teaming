# Active Directory Terminologies

## Object
An object can be defined as any resources present within an active directory environment like OUs, users, domain controllers etc

## Attributes

Every object in AD has an associated set of attributes used to define characteristics of the given object.
Such as: hostname and DNS name.
All attributes in AD have an associated `LDAP Name`. It can be used when performing `ldap queries`.

## Schema

The blueprint of any enterprise environment. It defines what types of objects can exist in the AD database and their associated attributes. For example, Users in AD belong to the class "user" and computer objects to "computer". 

NOTE: 
- When an object is created from a class - `instantation`
- When an object created from a specific class - `instance of that class`.

## Domain

A domain is a group of objects such as computers, users, OUs, groups etc. Domains can operate independently of one another or be connected via `trust relationships`

## Forest

A forest is a collection of domains. It is the topmost container and contains all the AD objects.
- It can contain multiple domans 
- May have various trust relatioships with other forests.

## Tree

A tree is a collection of AD domains that begins at a single root domain. A forest is a collection of AD trees. 
- A parent child trust is formed when a domain is added under another domain in a tree. 
- Two trees in the same forest cannot share a name (namespace)

## Container

Container objects hold other objects and have a defined in the directory subtree hierarchy.

## Leaf

Leaf objects do not contain other objects are found at the end of the subtree field

