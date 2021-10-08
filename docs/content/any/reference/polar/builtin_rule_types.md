---
title: Built-in Rule Types
any: true
weight: 3
description: |
  Reference for built-in rule types.
---

# Built-in Rule Types

Oso recognizes certain Polar rules as building blocks for implementing authorization best practices. Some of these rules are expanded from [shorthand rules](reference/polar/polar-syntax#shorthand-rules) in [resource blocks](reference/polar/polar-syntax#actor-and-resource-blocks), and others must be implemented in order to act as policy entry points for Oso's enforcement APIs.

To ensure consistent and correct usage of these building block rules, Oso ships with built-in [rule types](reference/polar/polar-syntax#rule-types) for each one.

The following rule types will be checked every time a policy is loaded:

## `has_permission`

```polar
type has_permission(actor: Actor, permission: String, resource: Resource);
type has_permission(actor: Actor, permission: String, resource: Actor);
```

`has_permission` rules grant actors permissions on resources. These rules can be generated by [shorthand rules](reference/polar/polar-syntax#shorthand-rules). The first argument must be a type of `Actor`, and the third argument must be a type of `Resource` or `Actor`, with both types declared as [actor or resource blocks](reference/polar/polar-syntax#actor-and-resource-blocks). The second argument must be a `String` and should be [declared as a permission](reference/polar/polar-syntax#permission-declarations) in the block for the third argument type.

## `has_role`

```polar
type has_role(actor: Actor, role: String, resource: Resource);
type has_role(actor: Actor, role: String, resource: Actor);
```

`has_role` rules grant actors roles on resources. These rules can be generated by [shorthand rules](reference/polar/polar-syntax#shorthand-rules). The first argument must be a type of `Actor`, and the third argument must be a type of `Resource` or `Actor`, with both types declared as [actor or resource blocks](reference/polar/polar-syntax#actor-and-resource-blocks). The second argument must be a `String` and should be [declared as a role](reference/polar/polar-syntax#role-declarations) in the block for the third argument type.


## `has_relation`

<!-- // TODO: revisit this when working on extension guides. This rule currently lets users define any relation they would like, but we may want to restrict that a bit more. -->
```polar
type has_relation(subject: Resource, relation: String, object: Resource);
type has_relation(subject: Resource, relation: String, object: Actor);
type has_relation(subject: Actor, relation: String, object: Actor);
type has_relation(subject: Actor, relation: String, object: Resource);
```

`has_relation` rules are used to look up relations between application objects. The first and third arguments can be any combination of `Resource` and `Actor` types declared as [actor or resource blocks](reference/polar/polar-syntax#actor-and-resource-blocks). The second argument must be a String and should be [declared as a relation](reference/polar/polar-syntax#relation-declarations) in the block for the third (`object`) argument type.


## `allow`

```polar
type allow(actor, action, resource);
```

`allow` rules are the top-level entrypoint for policy evaluation. These rules
are queried by the [resource-level enforcement API
methods](guides/enforcement/resource). This type of rule must have 3 arguments.

## `allow_field`

```polar
type allow_field(actor, action, resource, field);
```

`allow_field` rules are similar to `allow` rules, but they include the field
that is being accessed on the resource. These rules are queried by the
[field-level enforcement API methods](guides/enforcement/field). This type of
rule must have 4 arguments.

## `allow_request`

```polar
type allow_request(actor, request);
```

`allow_request` rules are similar to `allow` rules, but instead of authorizing
an action and a resource, they authorize access to a request. These rules are
queried by the [request-level enforcement API
methods](guides/enforcement/request). This type of rule must have 2 arguments.