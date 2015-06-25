#Abstract

This document describes recommendations for handling of custom SAML
attributes, that is attributes not defined in well know schema for use
within SAML identity federations in Research and Education.

[RFC generic stuff goes here]


#1. Introduction

Many SAML federations in Research and Education have standardised their attribute exchange using two well know schema: eduPerson [EDUPERSON] and Schac (SCHema for Academia) [SCHAC]. These schemas are commonly used, and work well for exchanging attributes that are generally derived from campus identity management systems.

However, use cases exist where these schema do not provide enough usable attribute definitions. These cases range from service providers wishing to define additional attributes to express licensing information, to collaborative organisations wanting to express additional attributes for authorisation purposes. These cases are typically so specific that it is unlikely that adding specific attributes to existing schema is going to provide a generic solution. Editors of the above schemas are therefore reluctant to add these attributes.

The eduPerson schema does provide an attribute called eduPersonEntitlement [EDUPERSON.edupersonEntitlement] in which the value of the entitlement indicates a set of rights to specific resources. The  structure of the value itself is however completely semantically free, which introduces challenges when using this attribute in multi-domain scenarios, like for example interfederation.

Another way of expressing roles and rights is via group memberships. For expressing a group membership in a SAML attribute, the IsMemberOf attribute [EDUMEMBER:ismemberof] exists. As with eduPersonEntitlement, this attribute does not take into account any kind of scoping.

For this purpose this document defines a RECOMMENDED scoped semantic for edupersonEntitlement and IsMemberOf attribute values.

##1.1.  REFEDS Document Process

The Research and Education Federations group ([REFEDS]) is the voice that articulates the mutual needs of research and education identity federations worldwide.  It aims to represent the requirements of research and education in the ever-growing space of access and identity management.

From time to time REFEDS will wish to publish a document in the Internet RFC series. Such documents will be published as part of the RFC Independent Submission Stream [RFC4844]; however the REFEDS working group sign-off process will have been followed for these documents, as described in the REFEDS Participant's Agreement [REFEDS.agreement].

This document is a product of the REFEDS Working Group process.

#2.  Notation and Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [BCP14].

#3. Scoped eduPersonEntitlement and IsMemberOf values

##3.1  Scoped eduPersonEntitlement values

###3.1.1 Syntax

eduPersonEntitlement values MUST be valid URIs.

###3.1.2 Semantics

####3.1.2.1 Scoping requirements

Custom attributes expressed in the eduPersonEntitlement attribute are commonly used for authorization proposes. As such a scoped version of the attribute value of an eduPersonentitlement should include a scope that:
* Works within established definition of the eduPersonentitlement attribute
* Includes the authoritative source for its value(s).
* Prevents clashes between attribute values for different services.
* Allows for easy filtering, both for adhering to attribute release policies by the Identity Provider as well as quickly determining authorization by the Service Provider.
* Allows for fine-grained authorization at the Service Provider.
* Should not impose unnecessary restrictions on the semantics of the actual attribute value to be transmitted.

####3.1.2.2 URNs versus URLs

Though technically all of the scoping requirements can be met using URLs, in practice it is easier to express the values as URNs as this allows for using generic regular expressions to filter attribute values. Therefore, to adhere to this profile a scoped eduPersonEntitlement value SHOULD be expressed as a URN [RFC3406]

###3.1.3 Generic format

####3.1.3.1 Scoped eduPersonEntitlements

A scoped eduPersonEntitlement value should have the following properties:
* it MUST contain the URN prefix,
* it MUST contain the namespace of the authoritative source for the entitlement as defined per [RFC3406],
* it MUST contain the name of the service being targeted, for clarity it is RECOMMENDED to express this as the FQDN of the service,
* it SHOULD contain the name of the entitlement,
* it MAY contain the value of the entitlement.

A generic example for a scoped eduPersonEntitlement SHOULD be formatted as follows:

	urn:[namespace]:[servicename]:[entitlementName]:[entitlementValue]

If only one attribute value needs to be provided or the presence of the attribute value itself suffices, the entitlementName property MAY be omitted as follows:

	urn:[namespace]:[servicename]:[entitlementValue]


####3.1.3.1 Service Provider Specific Entitlements

The common scenario when using eduPersonEntitlement is that a Service defines the value of the custom attribute it needs for its service.

  urn:[service namespace]:[servicename]:{[entitlementName]}:[entitlementValue]

Note that the Service namespace needs to be formally registered unless a prefix of x- is used to signify a custom namespace [RFC3406].

Examples:

	urn:mace:terena.org:tcs:personal-admin
	urn:x-surfnet:surfdomeinen.nl:role:dnsadmin


####3.1.3.2 Identity Provider Specific Entitlements

In this scenario the IdP defines the entitlement value. In general this is not very convenient, as this means the service will need to interpret each entitlement value on a per Identity Provider basis.

	  urn:[identity provider namespace]:[servicename]:{[entitlementName]}:[entitlementValue]

Note that the IDP namespace needs to be formally registered unless a prefix of x- is used to signify a custom namespace [RFC3406].

Examples:

	  urn:mace:exampleIdP.org:demoservice.com:demo-admin
	  urn:x-surfnet:surfnet.nl:authz:role:wikiadmin

##3.2 Scoped IsMemberOf values

###3.2.1 Syntax

IsMemberOf is multivalued, and currently has no controlled vocabulary

###3.2.2 Semantics

####3.2.2.1 Scoping requirements

Group memberships expressed in the IsMemberOf attribute can be used used for authorization proposes. As such a scoped version of the attribute value of an IsMemberOf should include a scope that:
* Works within established definition of the IsMemberOf attribute
* Includes the authoritative source for its value(s).
* Prevents clashes between attribute values for different services.
* Allows for easy filtering, both for adhering to attribute release policies by the Identity Provider as well as quickly determining authorization by the Service Provider.
* Allows for fine-grained authorization at the Service Provider.
* Should not impose unnecessary restrictions on the semantics of the actual attribute value to be transmitted.

###3.2.1.3 Generic format

Several attributes already exist that include scope in their values. It is proposed to use the same well understood mechanism for scoping the IsMemberOf values by using the format [localGroupIdentifier] @ [domain od authoratative source]. For example:

	  foo@eduteams.org

#4.  Security Considerations
This memo requires no addition security considerations beyond what was already specified in the Normative References

#5.  References

##5.1.  Normative References

   [BCP14] Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels ", BCP 14, RFC 2119, March 1997.

   [EDUPERSON] Internet2 Middleware Architecture Committee for Education, Directory Working Group (MACE-Dir), eduPerson Object Class Specification (201203),
	<http://www.internet2.edu/media/medialibrary/2013/09/04/internet2-mace-dir-eduperson-201203.html>

[EDUPERSON.eduPersonEntitlement] Internet2 Middleware Architecture Committee for Education, Directory Working Group (MACE-Dir), eduPerson Object Class Specification 	(201203),
<http://www.internet2.edu/media/medialibrary/2013/09/04/internet2-mace-dir-eduperson-201203.html#eduPersonEntitlement>

[SCHAC] SCHema for Academia, ,<http://www.terena.org/activities/tf-emc2/schacreleases.html>

[EDUMEMBER.ismemberof] https://www.switch.ch/aai/support/documents/attributes/ismemberof/index.html

[RFC3406] Uniform Resource Names (URN) Namespace Definition Mechanisms, <https://tools.ietf.org/html/rfc3406>

##5.2.  Informative References

   [REFEDS.agreement] Research and Education Federations, "REFEDS Participant's Agreement", <https://refeds.org/about/about_agreement.html>.

   [REFEDS] Research and Education Federations, "REFEDS Home Page", <http://www.refeds.org/>.

   [RFC4844]  Daigle, L. Internet Architecture Board, "The RFC Series and RFC Editor", RFC 4844, July 2007.
