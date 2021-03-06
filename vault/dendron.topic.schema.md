---
id: c5e5adde-5459-409b-b34d-a0d75cbb1052
title: Schema
desc: ''
updated: 1595952505039
created: 1595952505039
---
# Schemas


As you end up creating more notes, it can be hard to keep track of it all. This is why Dendron has **schemas** to help you manage your notes at scale. Think of schemas as an **optional type system** for your notes. They describe the hierarchy of your data and are themselves, represented as a hierarchy.

Schemas show up as icons next to lookup results.

![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/schema-closeup.jpg)

## Schema Anatomy

A schema is a YAML file with the following naming scheme `{name}.schema.yml`

Below is an example of a four-level hierarchy describing cli commands. The `id` field of the schema is used to build a [glob pattern](https://facelessuser.github.io/wcmatch/glob/) which will be used to match notes matching the schema.

```yml
# this will match "cli.*" notes
- id: cli 
  # human readable description of hierarchy
  desc: command line interface reference
  # add this to the domain of your schema hierarchy
  parent: root
  # when a schema is a namespace, it can have arbitrary children. equivalent to cli.* glob pattern
  namespace: true 
  children:
    - cmd
    - env
# will match cli.*.env
- id: env
  desc: cli specific env variables
# will match cli.*.cmd.*
- id: cmd
  desc: subcommands 
  namespace: true
```

Below is another way of representing the above schema
```
.
└── cli # matches cli
    └── {cli child} # matches cli.*
        ├── env # matches cli.*.env
        └── cmd # matches cli.*.cmd
            └── {cmd child} # matches cli.*.cmd.*
```

## Schema Properties
### id
the identifier of the schema. also designates the glob pattern match

### desc
human readable description of the schema node. these will automatically when creating notes that match the schema.

![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/schema-desc.gif)

### parent
only required for schema [domain](https://www.dendron.so/notes/c6fd6bc4-7f75-4cbb-8f34-f7b99bfe2d50.html#domain)

### namespace

designates this schema as a namespace meaning it will match any arbitrary child. equivalent to `{id}.*` glob pattern

### children

list of ids of schemas that are the child of the current schema

### template
a template you can apply to all notes that match this schema
```yml
template:
  # identifier for the template (for a note based template, it would be the name of the note)
  id: journal.template.daily
  # what sort of template we are creating. currently, only 'note' is supported
  type: note

```

## Schema Templates

Schema templates let you specify a pre-defined template that will automatically be applied to all notes that match the template. You can see a video of how this works below.

This is extremely useful whenver you want to re-use the outline of a note. Examples include daily journals, weekly shopping lists, and weekly reviews.  

NOTE: you'll need to run `> Dendron: Reload Index` whenever you updated your schema for the changes to take effect. 

<a href="https://www.loom.com/share/481b7ab051394c1caa383383bd265755"> 
<img style="" src="https://cdn.loom.com/sessions/thumbnails/481b7ab051394c1caa383383bd265755-with-play.gif"> 
</a>

## Stubs

Stubs are notes that don't exist but will show up in lookup results. There are two reasons why these notes might show up: 
- they are the parent of a note deeper in the hierarchy (eg. `foo.bar` might be a stub for `foo.bar.foobar`)
- they are possible notes according to the schema

![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/schema-plus.jpg)
> The `+` sign next to the suggestion indicates that the note is a stub and does not exist 

## Unknown Schema

Dendron doesn't force you to use schemas if you don't want to. This is why you can create notes that don't match any schema. Dendron will show you a `?` next to these results.

![](https://foundation-prod-assetspublic53c57cce-8cpvgjldwysl.s3-us-west-2.amazonaws.com/assets/images/schema-question.jpg)

> Sometimes ~~rules~~ schemas are meant to be broken

## Working with Schemas

Schemas can be modified, created and deleted using the same lookup interface that you use for regular notes. See [[lookup | dendron.topic.lookup]] for further details about working with schemas.